Report
===

### [awr Report](./awr.md)

<br>

### [ash Report](./ash.md)

<br>

### awr 주기 확인
```sql
SELECT * DBA_HIST_WR_CONTROL;
/*
DBID, SNAP_INTERVAL, RETENTION, TOPNSQL, CON_ID, SRC_DBID, SRC_DBNAME
1308564973	+00 01:00:00.000000	+08 00:00:00.000000	DEFAULT	0	1308564973	SID_NAME
*/
```

<br>

### awr 주기 설정
```sql
--RETENTION : 보관시간, INTERVAL : 주기
--설정단위 : 분
EXEC DBMS_WORKLOAD_REPOSITORY.MODIFY_SNAPSHOT_SETTINGS(RETENTION=>60*24*14,INTERVAL=>15);
```

<br>
