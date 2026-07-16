Flashback
===

### 요구 사항
1. UNDO_MANAGEMENT가 AUTO로 되어있을 것
1. UNDO_RETENTION 파라미터 값 지정
1. 유저가 Flashback을 사용하려면 DBMS_FLASHBACK에 대한 실행 권한이 있어야할 것

* 파라미터 값 확인
  ```sql
  SHOW PARAMETER UNDO;
  /*
  NAME                                 TYPE        VALUE
  ------------------------------------ ----------- ------------------------------
  _highthreshold_undoretention         integer     16200
  _optimizer_undo_cost_change          string      11.2.0.3
  undo_management                      string      AUTO
  undo_retention                       integer     2400
  undo_tablespace                      string      UNDOTBS1
  */

  --AUTO로 변경
  ALTER SYSTEM SET UNDO_MANAGEMENT = AUTO SCOPE = BOTH;
 
  --UNDO 테이블 스페이스 생성
  CREATE UNDO TABLESPACE tablespace_name DATAFILE 'directory\tablespace_name.dbf' SIZE size;

  --UNDO 테이블 스페이스 지정
  ALTER SYSTEM SET UNDO_TABLESPACE = tablespace_name;

  --UNDO_RETENTION 설정 (Seconds)
  ALTER SYSTEM SET UNDO_RETENTION = 1800;

  --DBMS_FLASHBACK 실행 권한 부여
  GRANT EXECUTE ON DBMS_FLASHBACK TO owner_name;
  ```

<br>

### 기본 형식
```sql
SELECT * FROM owner_name.table_name AS OF TIMESTAMP TO_TIMESTAMP('20220427 093824', 'YYYYMMDD HH24MISS')
WHERE ... ;
```

<br>

### 플래시백 트랜잭션 쿼리 확인
```sql
SELECT * FROM SYS.flashback_transaction_query;
```

<br>
