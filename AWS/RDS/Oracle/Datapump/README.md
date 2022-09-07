Datapump
===

>[https://docs.oracle.com/en/database/oracle/oracle-database/21/arpls/DBMS_DATAPUMP.html#GUID-84C5942C-B4CC-4128-8D7E-21EAC158F363](https://docs.oracle.com/en/database/oracle/oracle-database/21/arpls/DBMS_DATAPUMP.html#GUID-84C5942C-B4CC-4128-8D7E-21EAC158F363)
>[https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Oracle.Procedural.Importing.DataPump.html](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Oracle.Procedural.Importing.DataPump.html)

### expdp
```sql

```

<br>

### impdp
```sql
--Schema
DECLARE
  v_hdnl NUMBER;
BEGIN
  v_hdnl := DBMS_DATAPUMP.OPEN(operation => 'IMPORT', job_mode  => 'SCHEMA', job_name  => null);
  DBMS_DATAPUMP.ADD_FILE(handle => v_hdnl, filename  => 'dmpfile.dmp', directory => 'DATA_PUMP_DIR', filetype  => dbms_datapump.ku$_file_type_dump_file);
  DBMS_DATAPUMP.ADD_FILE(handle => v_hdnl, filename  => 'logfile.log', directory => 'DATA_PUMP_DIR', filetype  => dbms_datapump.ku$_file_type_log_file);
  
  --스키마 지정
  DBMS_DATAPUMP.METADATA_FILTER(v_hdnl,'SCHEMA_EXPR','IN (''schema_name1'',''schema_name2'' ... )');
  DBMS_DATAPUMP.START_JOB(v_hdnl);
END;
/

--Table
DECLARE
  v_hdnl NUMBER;
BEGIN
  v_hdnl := DBMS_DATAPUMP.OPEN(operation => 'IMPORT', job_mode  => 'TABLE', job_name  => null);
  DBMS_DATAPUMP.ADD_FILE(handle => v_hdnl, filename  => 'dmpfile.dmp', directory => 'DATA_PUMP_DIR', filetype  => dbms_datapump.ku$_file_type_dump_file);
  DBMS_DATAPUMP.ADD_FILE(handle => v_hdnl, filename  => 'logfile.log', directory => 'DATA_PUMP_DIR', filetype  => dbms_datapump.ku$_file_type_log_file);
  
  --테이블 통계 데이터 제외
  DBMS_DATAPUMP.METADATA_FILTER(v_hdnl, 'EXCLUDE_PATH_EXPR', 'IN (''TABLE_STATISTICS'')');
  
  --테이블이 이미 존재할 경우의 작업 지정
  DBMS_DATAPUMP.SET_PARAMETER(v_hdnl,'TABLE_EXISTS_ACTION', 'TRUNCATE');
  
  DBMS_DATAPUMP.START_JOB(v_hdnl);
END;
/
```

<br>

### Log 파일 확인
```sql
--Log 확인
SELECT text FROM table(rdsadmin.rds_file_util.read_text_file('DATA_PUMP_DIR','logfile.log'));
```

<br>
