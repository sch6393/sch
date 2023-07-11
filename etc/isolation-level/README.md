Isolation Level (격리 수준)
===

### 종류
* LEVEL 0 : READ UNCOMMITTED
  * 각 트랜젝션에서 변경된 내용을 COMMIT이나 ROLLBACK과 상관없이 다른 트랜잭션에서 볼 수 있음 (DIRTY READ)
  * 정합성에 문제가 많으므로 사용하지 않음

<br>

* LEVEL 1 : READ COMMITTED
  * 대부분의 RDBMS에서 사용하는 방식
  * DIRTY READ가 발생하지 않음 (실제 테이블 데이터를 가져오는게 아닌 UNDO 영역에 백업된 데이터를 가져오기 때문)
  * 트랜잭션 내에서의 똑같은 SELECT 쿼리로 조회했음에도 불구하고 다른 값이 나올 가능성이 있음 (NON-REPEATABLE READ)

<br>

* LEVEL 2 : REPEATABLE READ
  * MySQL InnoDB의 기본 격리 수준 레벨은 이 방식으로 설정되어 있음
  * 트랜잭션마다 번호를 할당하여 해당 번호보다 낮은 트랜잭션에서만 변경한 데이터를 읽게 함 (UNDO 영역에 데이터를 백업하고 실제 레코드 값은 변경됨)
    * 참고 - [MVCC](https://ja.wikipedia.org/wiki/MultiVersion_Concurrency_Control)
  * UNDO 영역에 백업된 데이터가 많아지면 성능 저하가 발생
  * 다른 트랜잭션에서 실행된 변경 작업에 의해 해당 레코드가 보이거나 안 보일 가능성이 있음 (PHANTOM READ)
  * PHANTOM READ를 방지하려면 쓰기 잠금이 필요

<br>

* LEVEL 3 : SERIALIZABLE
  * 가장 단순하고 엄격한 격리 수준
  * PHANTOM READ가 발생하지 않음
  * 처리 성능이 좋지 않기 때문에 특수한 상황 외에는 사용하지 않는 방식

<br>

|LEVEL|DIRTY READ|NON-REPEATABLE READ|PHANTOM READ|
|-|-|-|-|
|READ UNCOMMITTED|〇|〇|〇|
|READ COMMITTED|✕|〇|〇|
|REPEATABLE READ|✕|✕|〇|
|SERIALIZABLE|✕|✕|✕|