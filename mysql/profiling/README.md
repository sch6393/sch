Profiling
===

### 확인
```sql
SHOW VARIABLES LIKE '%profiling%';
/*
Variable_name, Value
have_profiling	YES
profiling	OFF
profiling_history_size	15    --저장할 프로파일링 갯수 (최대값 : 100)
*/
```

<br>

### 활성화
```sql
SET profiling = 1; --ON
SET profiling = 0; --OFF
```

<br>

### 프로파일링 값 확인
```sql
--쿼리 실행 후 프로파일링 값을 확인
SHOW PROFILES;

--쿼리 번호로 참조
SHOW PROFILE FOR QUERY 1;

--관련 정보만 참조
SHOW PROFILE CPU      FOR QUERY 1;
SHOW PROFILE BLOCK IO FOR QUERY 1;
SHOW PROFILE MEMORY   FOR QUERY 1;
SHOW PROFILE SOURCE   FOR QUERY 1;

--INFORMATION_SCHEMA 에서 확인
SELECT * AS DURATION FROM INFORMATION_SCHEMA.PROFILING WHERE QUERY_ID = 1 ORDER BY SEQ;
```

<br>

