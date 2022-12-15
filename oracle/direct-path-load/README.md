Direct Path Load
===

### 특징
* 일반 INSERT의 경우 테이블 LOGGING 설정과 상관없이 항상 데이터와 메타 데이터 변경에 대한 최대 REDO, UNDO 로그를 생성함
* Direct Path Load의 경우도 데이터와 메타 데이터 변경에 대한 REDO, UNDO 로그를 생성하지만 경우에 따라 다름
    1. 데이터 변경에 대한 UNDO 로그는 생성되지 않음 (메타 데이터 변경에 대한 UNDO 로그는 생성됨) 
    1. 데이터베이스가 ARCHIVE LOG 모드 또는 FORCE LOGGING 모드가 아니라면 각 테이블의 LOGGING 설정과 상관없이 데이터 변경에 따른 REDO 로그는 생성되지 않음
    1. 데이터베이스가 ARCHIVE LOG 모드이지만 FORCE LOGGING 모드는 아닐 경우 LOGGING 설정이 되어있다면 REDO 로그 생성, NOLOGGING 설정이라면 REDO 로그 생성을 하지 않음
    1. 데이터베이스가 ARCHIVE LOG 모드이면서 FORCE LOGGING 모드일 경우 LOGGING 설정과 상관 없이 데이터 변경에 따른 REDO 로그를 생성

<br>

### 장점
1. 다이렉트 로드이기 때문에 버퍼 캐시를 사용하지 않으므로 수행 시간이 단축됨
1. Parallel 설정도 가능하므로 한층 더 단축이 가능

<br>

### 단점
1. 실행 후 해당 테이블에 대해 동일한 트랜잭션에서 액세스를 할 수 없음. 액세스를 시도할 경우 [ORA-12838](../error/12838.md) 에러가 발생함
1. 파티션 테이블이 아닌 경우 HWM 이후 새로운 영역을 할당하므로 통상의 INSERT 보다 많은 영역을 사용할 수도 있음
1. Object 타입의 컬럼을 포함할 수 없음
1. 클러스터 테이블인 경우 사용할 수 없음

<br>
