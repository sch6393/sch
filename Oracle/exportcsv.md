Export csv
===

### .sql 파일 작성
```sql
set echo off
set pages 0
set trimspool on
set colsep ','
set lines 10000
set termout off
set feed off
set markup csv on

--Date Format
ALTER SESSION SET NLS_DATE_FORMAT = 'YYYY/MM/DD HH24:MI:SS';

spool /aaa/table_name.csv;

SELECT * FROM table_name;

spool off

quit
```
>~~`SELECT * FROM table_name;` 이런 쿼리라면 빈 공간이 전부 스페이스로 채워지는 현상이 생기기 때문에 쿼리를 변형해서 사용~~
<br>
`set markup csv on` 옵션을 사용하면 csv 포맷에 맞춰서 출력됨

<br>

### csv 추출
```sql
sqlplus -S user_name/password @sqlfile_name.sql
```

<br>

### 참고
* [sql 파일의 set 옵션](./SQLPlus.md#sql-plus-옵션)
* ~~추출 쿼리 작성~~ `set markup csv on` 옵션을 사용하면 해당 쿼리를 쓰지 않아도 됨
  ```sql
  SELECT 'SELECT'
  FROM DUAL
  UNION ALL
  SELECT '''"''||' || column_name || '||''",''||'
  FROM ALL_TAB_COLUMNS
  WHERE owner = 'owner_name' AND table_name = 'table_name'
  UNION ALL
  SELECT 'FROM owner_name.table_name;'
  FROM DUAL;

  --결과에서 마지막 컬럼의 ||'",'|| → ||'"'로 변경
  /*
  SELECT
  '"'||column_name1||'",'||
  '"'||column_name2||'",'||
  ...
  '"'||column_name8||'",'||
  '"'||column_name9||'",'||  →  '"'||column_name9||'"'
  FROM owner_name.table_name;
  */
  ```
