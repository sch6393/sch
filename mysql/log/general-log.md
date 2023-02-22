general log
===
>[https://dev.mysql.com/doc/refman/8.0/en/query-log.html](https://dev.mysql.com/doc/refman/8.0/en/query-log.html)

### 설정 확인
```sql
SHOW VARIABLES LIKE 'general_log%';
/*
Variable_name, Value
general_log	OFF
general_log_file	/directory_name/log/mysql-general.log
*/

--로그 저장 위치 확인
SHOW VARIABLES LIKE 'log_output';
/*
Variable_name, Value
log_output	TABLE
*/
```

<br>

### 설정 적용
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

### 쿼리 확인
```sql
--로그 저장 위치가 Table일 경우
SELECT * FROM mysql.general_log;
```

<br>
