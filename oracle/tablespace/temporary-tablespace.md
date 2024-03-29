Temporary Tablespace
===

### 확인
```sql
--사용 중인 크기 확인
SELECT * FROM DBA_TEMP_FREE_SPACE;
/*
TABLESPACE_NAME	TABLESPACE_SIZE	ALLOCATED_SPACE	FREE_SPACE	SHARED	INST_ID
TEMP	8589934592	244318208	8588886016	SHARED	​(NULL)​
*/

--기본값 확인
SELECT * FROM DATABASE_PROPERTIES WHERE PROPERTY_NAME = 'DEFAULT_TEMP_TABLESPACE';
/*
PROPERTY_NAME	PROPERTY_VALUE	DESCRIPTION
DEFAULT_TEMP_TABLESPACE	TEMP	Name of default temporary tablespace
*/

--기타 정보 확인
SELECT TABLESPACE_NAME, ROUND(SUM(BYTES) / (1024 * 1024 * 1024), 2) SUM_GB, ROUND(MAXBYTES / (1024 * 1024 * 1024), 2) MAX_GB, AUTOEXTENSIBLE
FROM DBA_TEMP_FILES GROUP BY TABLESPACE_NAME, MAXBYTES, AUTOEXTENSIBLE;
/*
TABLESPACE_NAME	SUM_GB	MAX_GB	AUTOEXTENSIBLE
TEMP	8	100	YES
*/
```

<br>

### 크기 변경
```sql
ALTER TABLESPACE temp_tablespace_name SHRINK SPACE KEEP 8G;

--MAX 값 변경
ALTER TABLESPACE temp_tablespace_name AUTOEXTEND ON MAXSIZE 100G;
```

<br>

### 생성, 삭제
```sql
CREATE TEMPORARY TABLESPACE temp_tablespace_name TEMPFILE SIZE 100M size;
DROP TABLESPACE temp_tablespace_name;
```

<br>

### Temporary 테이블스페이스 교체 순서
```sql
--1. 현재 사용 중인 Temporary 테이블스페이스 이름 확인
SELECT PROPERTY_VALUE FROM DATABASE_PROPERTIES WHERE PROPERTY_NAME = 'DEFAULT_TEMP_TABLESPACE';

--2. 변경할 Temporary 테이블스페이스 생성
CREATE TEMPORARY TABLESPACE temporary_tablespace_new TEMPFILE SIZE size;

--3. 변경
ALTER DATABASE DEFAULT TEMPORARY TABLESPACE_NAME temporary_tablespace_new;

--4. Temporary 테이블스페이스 변경 확인
SELECT PROPERTY_VALUE FROM DATABASE_PROPERTIES WHERE PROPERTY_NAME = 'DEFAULT_TEMP_TABLESPACE';

--5. 기존 Temporary 테이블스페이스 제거
DROP TABLESPACE temporary_tablespace_old;
```
>[3. 변경에서 RDS의 경우](../../aws/rds/oracle/temporary-tablespace/README.md)<br>
[5. 기존 Temporary 테이블스페이스 제거가 되지 않을 경우](../error/60100.md)

<br>
