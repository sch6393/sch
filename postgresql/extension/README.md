Extension
===

### 확인
```sql
\dx
/*
                                            List of installed extensions
        Name        | Version |   Schema   |                              Description
--------------------+---------+------------+------------------------------------------------------------------------
 pg_buffercache     | 1.3     | public     | examine the shared buffer cache
 pg_stat_statements | 1.8     | public     | track planning and execution statistics of all SQL statements executed
 pgcrypto           | 1.3     | public     | cryptographic functions
 plpgsql            | 1.0     | pg_catalog | PL/pgSQL procedural language
(4 rows)
*/

-- 설치가 가능한 확장 목록
SELECT * FROM PG_AVAILABLE_EXTENSIONS;
/*
        name        | default_version | installed_version |                                comment
--------------------+-----------------+-------------------+------------------------------------------------------------------------
 plpgsql            | 1.0             | 1.0               | PL/pgSQL procedural language
 dblink             | 1.2             |                   | connect to other PostgreSQL databases from within a database
 pg_stat_statements | 1.8             | 1.8               | track planning and execution statistics of all SQL statements executed
 pgcrypto           | 1.3             | 1.3               | cryptographic functions
 pg_buffercache     | 1.3             | 1.3               | examine the shared buffer cache
(5 rows)
*/

-- 설치가 되어 있는 확장 목록
SELECT * FROM PG_EXTENTION;
/*
  oid  |      extname       | extowner | extnamespace | extrelocatable | extversion | extconfig | extcondition
-------+--------------------+----------+--------------+----------------+------------+-----------+--------------
 13651 | plpgsql            |       10 |           11 | f              | 1.0        |           |
 23984 | pg_stat_statements |       10 |         2200 | t              | 1.8        |           |
 23998 | pgcrypto           |       10 |         2200 | t              | 1.3        |           |
 24035 | pg_buffercache     |       10 |         2200 | t              | 1.3        |           |
(4 rows)
*/
```

<br>

### 설치
```sql
-- 확장 설치
CREATE EXTENSION extension_name;

-- PostgreSQL을 수동 설치 했을 경우
-- 1. PostgreSQL을 설치할 때 사용한 소스 디렉토리/contrib 으로 이동
cd /usr/local/src/postgresql-16/contrib

-- 2. 해당 확장 디렉토리로 이동
cd extension_name

-- 3. Make, Install
make -f Makefile
make install

-- 패키지로 설치 했을 경우
dnf install postgresql-contrib
```

<br>

### 삭제
```sql
DROP EXTENSION extension_name;
```

<br>

### AWS RDS PostgreSQL에서 지원하는 확장
>https://docs.aws.amazon.com/AmazonRDS/latest/PostgreSQLReleaseNotes/postgresql-extensions.html

<br>
