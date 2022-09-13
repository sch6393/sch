Explain
===

### 확인
```sql
--실행계획 명시
EXPLAIN PLAN FOR
SELECT * FROM owner_name.table_name;

--실행계획 확인
SELECT * FROM TABLE(dbms_xplan.display);
```

<br>

### `'PLAN_TABLE' is old version` 라는 메시지가 있을 경우
```sql
--데이터 마이그레이션 과정 중 이전 버전의 테이블을 생성했다면 해당 메시지가 표시
--삭제하면 다시 생성됨
DROP TABLE PLAN_TABLE;
```

<br>

### 더 자세한 정보 확인 ➞ [Trace](../Trace/README.md)


