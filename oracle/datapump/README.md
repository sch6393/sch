Datapump
===

### expdp 기본 형식
```sql
--디렉토리 설정 확인
SELECT * FROM ALL_DIRECTORIES;

--Export 디렉토리 생성
CREATE OR REPLACE DIRECTORY directory_name as '/AAA/BBB';

--디렉토리 제거
DROP DIRECTORY directory_name;

--Export 명령 실행
--Schema 단위
expdp id/pw SCHEMAS=schema_name DIRECTORY=directory_name DUMPFILE=dumpfile.dmp LOGFILE=logfile.log

--Table 단위
expdp id/pw TABLES=schema_name.table_name DIRECTORY=directory_name DUMPFILE=dumpfile.dmp LOGFILE=logfile.log

--DB Link 연결
expdp id/pw NETWORK_LINK=dblink_name TABLES=schema_name.table_name DIRECTORY=directory_name DUMPFILE=dumpfile.dmp LOGFILE=logfile.log
```

<br>

### expdp 옵션
1. Export 단위 설정
    ```sql
    --전체
    FULL=Y

    --테이블 스페이스 단위
    TABLESPACES=tablespace_name

    --Schema 단위
    SCHEMAS=schema_name

    --Table 단위
    TABLES=schema_name.table_name
    ```
1. 데이터 범위 설정
    ```sql
    --테이블 레코드 데이터
    CONTENT=DATA_ONLY

    --테이블 정의 데이터
    CONTENT=METADATA_ONLY

    --테이블 전체 (기본값)
    CONTENT=ALL

    --제외할 데이터 (TABLE, INDEX, CONSTRAINT, GRANT)
    EXCLUDE=TABLE:"IN ('table_name1','table_name2' ... )"

    --데이터 범위 지정
    QUERY=owner_name.table_name:"where column_name > 10"
    
    --쉘에서 명령어를 실행하는 경우 특수 문자 앞에 \ (Backslash) 문자가 들어가야함
    EXCLUDE=TABLE:\"IN \(\'table_name1\', \'table_name2\'\)\"
    QUERY=owner_name.table_name:\"where date_column \>\= TO_DATE\(\'20220101\',\'YYYYMMDD\'\)\"
    ```
1. 기타 옵션
    ```sql
    --압축할 데이터 지정
    COMPRESSION=ALL | DATA_ONLY | METADATA_ONLY | NONE

    --갱신 내용 표시 (ATTACH의 STATUS에 설정된 시간 간격으로 갱신, 기본값은 실시간)
    STATUS

    --병렬 처리 지정 (단 지정된 갯수 만큼 데이터 파일을 만들어줘야 함)
    PARALLEL=4

    --%U로 자동으로 갯수를 맞춰줄 수 있음
    expdp id/pw SCHEMAS=schema_name DIRECTORY=directory_name DUMPFILE=dumpfile_%U.dmp PARALLEL=4

    --해당 작업에 특정 이름 지정
    JOB_NAME=job_name

    --위에서 지정한 이름으로 접근이 가능
    ATTACH id/pw JOB_NAME=job_name

    --ATTACH 접근 후 사용할 수 있는 옵션
    ADD_FILE  : 덤프 파일 추가
    PARALLEL  : 병렬 처리 지정
    KILL_JOB  : 작업 삭제
    START_JOB : 작업 재시작
    STOP_JOB  : 작업 중단
    EXIT      : 나가기
    STATUS    : 갱신 시간 지정
    ```

<br>

### Duplication 상태에서의 expdp
```sql
--PRIMARY에서 STANDBY의 디비 링크 생성
--CREATE PUBLIC DATABASE LINK STANDBY CONNECT TO SYSTEM IDENTIFIED BY abcd1234 USING 'STANDBY';
CREATE PUBLIC DATABASE LINK database_link_name CONNECT TO remote_user_name IDENTIFIED BY remote_password USING 'tnsname.ora_alias_name';

--확인
--SELECT SYSDATE FROM DUAL@STANDBY;
SELECT SYSDATE FROM DUAL@database_link_name;

--PRIMARY에서 STANDBY로 원격 접속으로 expdp
--expdp NETWORK_LINK=STANDBY SCHEMAS=ABCD DIRECTORY=directory_name DUMPFILE=dumpfile_name.dmp LOGFILE=logfile_name.log
expdp NETWORK_LINK=database_link_name SCHEMAS=schema_name DIRECTORY=directory_name DUMPFILE=dumpfile_name.dmp LOGFILE=logfile_name.log
--Username: / as sysdba
```

<br>

### impdp 기본 형식
```sql
--디렉토리 설정 확인
SELECT * FROM ALL_DIRECTORIES;

--Import 디렉토리 생성
CREATE OR REPLACE DIRECTORY directory_name as '/AAA/BBB';

--디렉토리 제거
DROP DIRECTORY directory_name;

--Export 명령 실행
--Schema 단위
impdp id/pw DIRECTORY=directory_name DUMPFILE=dumpfile.dmp SCHEMAS=schema_name TABLE_EXISTS_ACTION=REPLACE LOGFILE=logfile.log

--Table 단위
impdp id/pw DIRECTORY=directory_name DUMPFILE=dumpfile.dmp TABLES=schema_name.table_name TABLE_EXISTS_ACTION=REPLACE LOGFILE=logfile.log
```

<br>

### impdp 옵션
1. Import 단위 설정
    ```sql
    --전체
    FULL=Y

    --테이블 스페이스 단위
    TABLESPACES=tablespace_name

    --테이블 스페이스가 다른 경우 (Export한 테이블 스페이스 → Import할 테이블 스페이스)
    REMAP_TABLESPACE=tablespace_name:tablespace_name

    --Schema 단위
    SCHEMAS=schema_name

    --Schema가 다른 경우 (Export한 Schema → Import할 Schema)
    REMAP_SCHEMA=schema_name:schema_name

    --Table 단위
    TABLES=schema_name.table_name
    ```
1. 데이터 범위 설정
    ```sql
    --테이블 레코드 데이터
    CONTENT=DATA_ONLY

    --테이블 정의 데이터
    CONTENT=METADATA_ONLY

    --테이블 전체 (기본값)
    CONTENT=ALL

    --테이블이 이미 존재하는 경우 (SKIP, TRUNCATE, APPEND, REPLACE)
    TABLE_EXISTS_ACTION=SKIP

    --제외할 데이터 (TABLE, INDEX, CONSTRAINT, GRANT)
    EXCLUDE=INDEX
    ```
1. 기타 옵션
    ```sql
    --병렬 처리 지정 (단 지정된 갯수 만큼 데이터 파일을 지정해줘야 함)
    PARALLEL=4

    --%U로 자동으로 지정할 수 있음
    impdp id/pw DIRECTORY=directory_name DUMPFILE=dumpfile_%U.dmp PARALLEL=4
    ```

<br>

### 남은 시간 확인
```sql
SELECT SID, SERIAL#, OPNAME, SOFAR, TOTALWORK
FROM V$SESSION_LONGOPS
WHERE
    OPNAME = 'DATAPUMP2'
    AND SOFAR <> TOTALWORK
;
```

<br>
