Session
===

### 확인
```sql
SHOW PARAMETER SESSIONS;
/*
NAME                         TYPE    VALUE 
---------------------------- ------- ----- 
java_max_sessionspace_size   integer 0     
java_soft_sessionspace_limit integer 0     
license_max_sessions         integer 0     
license_sessions_warning     integer 0     
sessions                     integer 2500  
shared_server_sessions       integer
*/

SELECT * FROM V$SESSION;

SELECT
  S.OSUSER, S.USERNAME, S.SID, S.SERIAL#, S.MACHINE, S.PROGRAM, S.STATUS, S.LOGON_TIME, S.LAST_CALL_ET, A.SQL_TEXT
FROM V$SESSION S, V$SQLAREA A
WHERE
  S.SQL_ADDRESS = A.ADDRESS
  AND S.PREV_SQL_ID = A.SQL_ID
  AND S.LOGON_TIME > SYSDATE - 1 --세션이 생성되고 1일이 넘었을 경우
  AND S.LAST_CALL_ET > 120       --세션이 비활성화된지 120초가 넘었을 경우
;
```

<br>

### 최대 접속 세션 수
* SESSIONS = (1.5 * PROCESSES) + 22
  >[https://docs.oracle.com/cd/E18283_01/server.112/e17110/initparams229.htm](https://docs.oracle.com/cd/E18283_01/server.112/e17110/initparams229.htm)
* RDS의 경우 16GB 기준으로 약 1300개
  >[Quotas](../../aws/rds/oracle/quotas/README.md)

<br>

### 세션 강제 끊기
```sql
--SID, SERIAL#
ALTER SYSTEM KILL SESSION '2564, 40213';

--RDS
EXEC rdsadmin.rdsadmin_util.kill(2564, 40213);
```

<br>

### 참고
* [Process](../process/README.md)

<br>
