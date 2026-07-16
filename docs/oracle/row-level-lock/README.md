Row Level Lock
===

### 특징
* 오라클의 Row Level Lock은 중앙 집중형 관리 방식이 아님. 예를 들어 Table Lock인 TM Lock은 "어떤 테이블에 대해 어떤 세션이 어떤 모드로 Lock을 점유하고 있음" 이라는 정보를 중앙 집중형으로 관리함 (`SELECT * FROM V$LOCK;`)
* 오라클의 Row Level Lock의 점유 여부는 데이터가 존재하는 Row와 Block내에 같이 존재하기 때문에 특정 트랜잭션이 어떤 Row를 지금 변경했는지 안 했는지는 무조건 해당 Row를 찾아봐야만 알 수 있음
* 이러한 이유로 오라클은 어떤 Row를 변경해도 Row Level Lock을 Table Level Lock으로 확장되지 않음 (Escalation 되지 않음)
* 타 DBMS의 경우는 특정 트랜잭션이 어떤 Row를 변경했는지를 중앙 집중형으로 관리하기 때문에 Lock Escalation이 필수가 됨. 예를 들어 SQL Server의 경우 별도의 설정을 하지 않는 이상 하나의 테이블에 대해서 Row 변경이 일어난다면 Row Level Lock을 Table Level Lock으로 Escalation이 됨. 그러므로 동시성에는 성능이 좋지 않음
* 오라클의 Row Level Lock 구현은 동시성에 있어서는 성능이 좋지만 커밋 시점에서 문제가 발생할 수 있음. 예를 들어 100만건의 Row를 변경한 후 커밋을 한다고 했을 때 타 DBMS의 경우에는 이미 Table Level Lock으로 Escalation 되버렸기 때문에 Table Level에서만 커밋 처리를 하면 완료가 됨. (트랜잭션 종료를 기록) 오라클은 개별 Row와 Block마다 트랜잭션 정보를 가지고 있으므로 이를 모두 기록해주어야 함. 이러한 이유로 성능 저하를 막기 위해 트랜잭션 종료 처리를 딜레이 시키는 방식을 사용함
* 딜레이된 트랜잭션 종료 처리는 후에 해당 Block과 Row를 방문하는 세션에 의해 처리가 됨. 커밋 시에 트랜잭션 종료 처리하는 것을 Commit Cleanout, 나중에 처리하는 것을 Delayed Block Cleanout이라고 함. 오라클을 이 2개를 모두 사용

|장점|단점|
|-|-|
|절대로 Lock Escalation을 하지 않기 때문에 동시성에서 성능이 좋음|트랜잭션 종료 처리를 별도로 실행시켜줘야 하기 때문에 이 과정에서 불필요한 성능 저하가 발생할 수 있음|

<br>

### 참고
* [ORA-01555](../error/01555.md)

<br>
