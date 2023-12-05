INSERT INTO ~ ON DUPLICATE KEY UPDATE
===

### 설명
INSERT 작업 시 기존 PK와 충돌이 있을 경우 기존 데이터를 UPDATE 시키는 기능

<br>

### 조건
* PK 이외의 UK가 하나 이상 있어야 함

<br>

### 방법
```sql
INSERT INTO schema_name.table_name (column_name1, column_name2, ... ) VALUES (value1, value2, ... ) ON DUPLICATE KEY UPDATE column_name = new_value
;
```

<br>

### 작동 방식
1. INSERT로 데이터를 먼저 입력
1. UK에 해당하는 중복 값을 확인
  * 중복되는 UK 값이 존재하는 경우
    1. 지정된 컬럼을 UPDATE
    1. INSERT로 입력되었던 데이터를 DELETE
  * 중복되는 UK 값이 존재하지 않을 경우
    1. INSERT된 데이터를 유지
>위 작동 방식으로 인해 중복값이 있었다면 시퀸스 값이 증가하게 됨

<br>

### 문제점
DML이 많이 발생하는 경우 Deadlock이 자주 발생할 가능성이 있음. UPDATE를 할 때 먼저 컬럼 값을 읽은 후 Shared Lock (다른 트랜잭션에서 값이 변경되는 것을 방지) 을 얻고 컬럼에 값 갱신을 할 때 Exclusive Lock을 얻게 되는데 여러 세션에서 같은 Row에 서로 Exclusive Lock을 획득하려고 하기 때문

<br>
