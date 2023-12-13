Mysql High Availability
===
>https://github.com/yoshinorim/mha4mysql-manager
>https://github.com/yoshinorim/mha4mysql-node

### 개요
[Master Replication](../master-replication/README.md) 구조에서 데이터베이스 장애 발생 시 수동으로 이뤄지는 Master 변경 작업을 자동화하는 기능

<br>

### 지원하는 기능
1. Master 데이터베이스에 문제가 생긴 경우 (네트워크 에러, 프로세스 에러 등) 페일오버
1. 페일오버 시 기존 Master 데이터베이스와 새로운 Master 데이터베이스 간 최대한의 정합성을 보장
1. Slave 데이터베이스 간 적용된 [binary-log](../../log/binary/README.md) 차이를 확인하여 정합성 보장
1. 수동 스위치 오버, `1:N` 지원
1. 복제 모니터링
1. [GTID](../../gtid/README.md), Multi-Threaded Slave 지원

<br>

### 주의점
1. [Semi-sync Replication](../semi-sync-replication/README.md) 기능 활성화를 해야 좀 더 안전하게 일관성을 보장할 수 있음

<br>

### 페일오버 절차
1. Master에서 장애가 발생하면 MHA가 감지
1. Slave 중 가장 최신 Slave를 선택해 Master로 승격 (승격 시 아래의 데이터를 참조하여 대상을 선정함)
   1. `SHOW SLAVE STATUS\G;` 와 각 Slave의 Relay log를 확인
   1. Relay log에서 `end_log_pos` 값을 비교

1. Slave 중 데이터 복제가 가장 느린 Slave (i)에서 SQL 스레드가 Relay log에 기록된 모든 이벤트 실행 (Executes) 하게 되고 이 과정이 끝날 때까지 다음 과정은 대기
1. Slave (i)가 적용한 Master의 로그 파일, 로그 포지션 정보를 최신 Slave (Master로 승격된 데이터베이스) 가 읽은 바이너리 로그 파일, 로그 포지션을 비교해 차이가 발생한다면 그 부분의 트랜잭션을 Slave (i)에 반영
1. 최신 Slave (Master로 승격된 데이터베이스) 에서 반영된 바이너리 로그 포지션 이후 부터 장애가 발생된 시점의 마지막 Master의 바이너리 로그 포지션 차이를 적용하면 장애 시점의 Master의 데이터 시점까지 복구됨. 장애가 발생한 Master의 바이너리 로그 포지션 차이를 적용하면 모든 Slave는 장애 시점의 Master 데이터까지 복구됨
1. 위 순서는 Master 서버에 접근이 가능하고 장애 시점의 Binlog를 읽을 수 있는 경우에 수행됨. 만약 서버 자체에 문제가 생겨 접속이 불가능할 경우 5번은 수행할 수 없음       

<br>
