Log
===

### general log
쿼리 실행 내역을 파일이나 테이블로 기록하여 확인
```sql
--설정 확인
SHOW VARIABLES LIKE 'general_log%';

/*
Variable_name, Value
general_log	OFF
general_log_file	/rdsdbdata/log/general/mysql-general.log
*/

--로그 저장 위치 확인
SHOW VARIABLES LIKE 'log_output';
/*
Variable_name, Value
log_output	TABLE
*/

--로그 저장 위치가 Table일 경우
SELECT * FROM mysql.general_log;
```

<br>

### slow_log
실행시간이 오래 걸리는 쿼리를 파일이나 테이블로 기록하여 확인
```sql
--설정 확인
SHOW VARIABLES LIKE 'slow_query_%';

/*
Variable_name, Value
slow_query_log	ON
slow_query_log_file	/rdsdbdata/log/slowquery/mysql-slowquery.log
*/

--로그 저장 위치 확인
SHOW VARIABLES LIKE 'log_output';
/*
Variable_name, Value
log_output	TABLE
*/

--기준 시간 설정 확인 (초)
SHOW VARIABLES LIKE 'long%';
/*
Variable_name, Value
long_query_time	10.000000
*/

--로그 저장 위치가 Table일 경우
SELECT * FROM mysql.slow_log;
```

<br>

### 설정
```sql
--ON/OFF
SET GLOBAL general_log = ON;
SET GLOBAL general_log = OFF;

--Table 기록/File 기록 : 기록 위치를 변경하기 위해선 먼저 OFF가 되어야 함
SET GLOBAL log_output = 'TABLE';
SET GLOBAL log_output = 'FILE';
SET GLOBAL log_output = 'TABLE,FILE';
```

<br>
