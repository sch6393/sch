Datapump
===

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
  DBMS_DATAPUMP.METADATA_FILTER(v_hdnl,'SCHEMA_EXPR','IN (''schema_name'')');
  DBMS_DATAPUMP.START_JOB(v_hdnl);
END;
/

--Log 확인
SELECT text FROM table(rdsadmin.rds_file_util.read_text_file('DATA_PUMP_DIR','logfile.log'));
```

<br>


