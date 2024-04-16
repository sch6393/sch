Router
===

### 내용
InnoDB 클러스터의 구성요소이며 애플리케이션과 MySQL 사이의 라우팅 역할을 하는 미들웨어

<br>

### 특징
* 쿼리에 대한 로드 밸런스 (부하분산)
* 자동 페일 오버
* InnoDB 클러스터의 구성 변경 감지

<br>

### 전체적인 동작
1. MySQL 라우터는 애플리케이션과 MySQL 간에 위치해 동작하며 애플리케이션은 일반적인 MySQL에 연결하는 것처럼 라우터로 연결
1. 애플리케이션이 MySQL 라우터에 연결할 때마다 라우터는 구성되어 있는 인스턴스 리스트에서 적절한 MySQL을 선택, 연결
1. 연결한 순간부터 라우터는 응답을 포함해 애플리케이션과 MySQL 간의 모든 네트워크 트래픽을 담당
1. MySQL 라우터는 MySQL의 캐시 목록 로드 및 갱신, 구성된 InnoDB 클러스터의 네트워크 연결 (토폴로지) 및 유지함
1. 캐시를 갱신된 상태로 유지하기 위한 메타 데이터 캐시 구성 요소는 메타 데이터가 포함된 InnoDB 클러스터 중 하나에 대해 오픈 커넥션을 유지함 (MySQL의 Performance 스키마에서 쿼리를 수행해 메타 데이터 데이터베이스 및 라이브 상태 정보를 확인)
1. 클러스터 메타 데이터는 MySQL Shell을 사용하여 MySQL 추가, 제거 등 InnoDB 클러스터가 수정될 때마다 변경되며 Performance_schema 테이블은 클러스터 상태 변경이 감지될 때마다 MySQL의 Group Replication 플러그인에 의해 실시간으로 갱신됨
1. 예를 들어 MySQL 하나가 예기치 않게 종료된 경우 메타 데이터 캐시 연결이 끊어지고 다시 연결할 수 없기 때문에 라우터는 다른 MySQL에 연결, 메타 데이터 및 InnoDB 클러스터 상태를 확인하려고 함
1. 종료된 MySQL에 대한 애플리케이션 연결은 자동으로 닫히게 되고 그 후 라우터에 다시 연결할 경우 다른 MySQL로 연결이 이루어짐

<br>

### 파라미터
* `[DEFAULT] dynamic_state` : InnoDB 클러스터 메타데이터 서버 주소를 추적 및 저장함. 라우터가 재기동되면 다시 로드<br>
파일명은 `state.json` 이고 해당 파일은 클러스터를 구성하는 MySQL 인스턴스 정보를 확인할 수 있음
    ```json
    {
        {
        "metadata-cache": {
            "group-replication-id": "cbe9b50c-26a5-11ee-ab5a-0a8b43dec245",
            "cluster-metadata-servers": [
                "mysql://host1:3306",
                "mysql://host2:3306",
                "mysql://host3:3306"
            ]
        },
        "version": "1.0.0"
    }
    ```

* `[metadata_cache] ttl` : 캐시되어 있는 메타 데이터에 대한 갱신 주기 설정
* `[metadata_cache] use_gr_notifications` : 그룹 복제 (Group Replication) 에 발생하는 변경 사항에 대해서 MySQL Router가 알림 받는 내용에 대한 설정<br>해당 옵션은 8.0.17 부터 추가된 기능이며 활성화되면 MySQL Router는 대부분의 클러스터 변경 사항에 대해 비동기식으로 알림을 받게 되고 MySQL Router는 그룹 복제에서 아래의 알림을 받는다면 클러스터 메타 데이터를 갱신함
    ```
    group_replication/membership/quorum_loss
    group_replication/membership/view
    group_replication/status/role_change
    group_replication/status/state_change
    ```
    그룹 복제 알림 기능을 사용하려면 X 프로토콜 연결이 되어야 함. X 프로토콜 연결이 되지 않으면 설정된 TTL 간격으로 메타 데이터를 갱신함

* `[routing] destinations` : 애플리케이션으로부터 MySQL Router로 쿼리 요청을  받을 경우 전달해야하는 목적지 서버 정보 (또는 클러스터) 를 설정하는 옵션이며 정적 (IP, 호스트명) 과 동적 (클러스터명) 둘 다 가능함<br>`metadata-cache://cluster_name/?role`  을 통해 `PRIMARY`, `SECONDARY`, `PRIMARY_AND_SECONDARY` 선택 가능
* `[routing] route_strategy` : 특정 라우팅 방식을 설정할 수 있으며 `first-available`, `next-available`, `round-robin`, `round-robin-with-fallback` 선택 가능

<br>

### 라우팅 방식
|라우팅 방식|설명|
|-|-|
|`first-available`|새 연결이 있을 경우 사용 가능한 서버들 중 가장 첫번째 서버로 라우팅. 오류가 발생했다면 그 다음 서버로 라우팅.|
|`next-available`|`first-available` 과 같이 새 연결이 있을 경우 사용 가능한 서버들 중 가장 첫번째 서버로 라우팅하지만 오류가 발생했다면 오류가 발생한 서버는 라우팅 대상 서버에서 아예 제외되는 특징을 가짐. 라우터를 재기동하면 서버 리스트 초기화|
|`round-robin`|[`round-robin`](../../etc/cpu-scheduling/README.md#preemptive-알고리즘) 로직으로 Primary, Secondary 관계 없이 사용 가능한 서버를 사용|
|`round-robin-with-fallback`|`round-robin` 과 같은 로직으로 동작하지만 Secondary 서버만 사용함. Secondary 서버에 오류가 있다면 Primary 서버 리스트를 사용|
>https://dev.mysql.com/doc/mysql-router/8.0/en/mysql-router-conf-options.html#option_mysqlrouter_routing_strategy

<br>
