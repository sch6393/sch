Parameter
===
>[19c (English, PDF)](https://docs.oracle.com/cd/F19136_01/refrn/database-reference.pdf)

>[19c (English, Web Page)](https://docs.oracle.com/en/database/oracle/oracle-database/19/refrn/index.html#Oracle%C2%AE-Database)

>[19c (日本語、Web Page)](https://docs.oracle.com/cd/F19136_01/refrn/index.html#Oracle%C2%AE-Database)

### 파라미터 확인
```sql
SHOW PARAMETER parameter_name;

SELECT * FROM V$PARAMETER;   --현재 세션에 유효한 초기화 파라미터 정보
SELECT * FROM V$PARAMETER2;  --현재 세션에 유효한 초기화 파라미터 정보 (각 목록 파라미터 값은 행으로 표시)
SELECT * FROM V$SPPARAMETER; --SPFILE에 대한 정보 표시 (SPFILE을 사용하지 않고 기동한 경우 ISSPECIFIED에 FALSE가 포함됨 )
```

<br>

### 파라미터 수정
```sql
ALTER SYSTEM SET parameter_name=value SCOPE=BOTH;

/*
* SCOPE 옵션값
* SPFILE : SPFILE만 수정
* MEMORY : 가동 중인 데이터베이스의 Parameter 값만 수정하고 SPFILE은 수정하지 않음 (데이터베이스 재가동 시 설정하기 전 값으로 변경됨)
* BOTH   : SPFILE과 데이터베이스에 설정된 Parameter 값 양쪽 모두 수정 (DEFAULT)
*/
```

<br>

### [db_block_%](./parameter/db_block.md)
### [db_file_multiblock_read_count](./parameter/db_file_multiblock_read_count.md)
### [db_writer_processes](./parameter/db_writer_processes.md)
### [dbwr_io_slaves](./parameter/dbwr_io_slaves.md)
### [pool_size_%](./parameter/pool_size.md)
### [sec_case_sensitive_logon](./parameter/sec_case_sensitive_logon.md)
### [session_cached_cursors](./parameter/session_cached_cursors.md)
### [sort_area_size](./parameter/sort_area_size.md)

<br>
