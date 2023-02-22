slow query
===
>[https://dev.mysql.com/doc/refman/8.0/en/slow-query-log.html](https://dev.mysql.com/doc/refman/8.0/en/slow-query-log.html)

### 설정 확인
```sql
SHOW VARIABLES LIKE 'slow_query_%';
/*
Variable_name, Value
slow_query_log	ON
slow_query_log_file	/directory_name/log/mysql-slowquery.log
*/

--로그 저장 위치 확인
SHOW VARIABLES LIKE 'log_output';
/*
Variable_name, Value
log_output	TABLE
*/

--기준 시간 설정 확인 (초)
SHOW VARIABLES LIKE 'long_query_time';
/*
Variable_name, Value
long_query_time	10.000000
*/
```

<br>

### 설정 적용
```sql
--ON/OFF
SET GLOBAL slow_query_log = ON;
SET GLOBAL slow_query_log = OFF;

--Table 기록/File 기록 : 기록 위치를 변경하기 위해선 먼저 OFF가 되어야 함
SET GLOBAL log_output = 'TABLE';
SET GLOBAL log_output = 'FILE';
SET GLOBAL log_output = 'TABLE,FILE';
```

<br>

### 쿼리 확인
```sql
--로그 저장 위치가 테이블인 경우
SELECT * FROM mysql.slow_log;
```

<br>
