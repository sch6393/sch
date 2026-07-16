Statspack
===

### 정의
Staspack은 오라클 데이터베이스에 대한 부하, 리소스 사용량, 트랜드 분석 등 성능 문제 분석을 위해 사용되는 툴이며 예전 `UTLBSTAT`, `UTLESTAT` 가 제공하던 기능이며 이를 수정, 보완한게 Statspack

성능관련 통계정보들이 `PERFSTAT` 유저에 누적되어 저장되므로 원하는 기간별로 비교 분석이 가능하고 `DBMS_JOB` 이나 크론 같은 걸 이용해 주기적인 데이터 수집도 가능

특정 시점의 데이터들의 집합을 스냅샷이라고 하며 Statspack 레포트는 두 개의 스냅샷으로부터 작성됨

또한 ASH, AWR 레포트는 유료 옵션이지만 Statspack 레포트는 무료 (AWR처럼 Top 5 Timed Event 및 메모리 Advisory 확인도 가능)

<br>

### 설치
```sql
--(옵션) 1. PERFSTAT 전용 테이블스페이스 생성
CREATE TABLESPACE tablespace_name DATAFILE '/directory_name/tablespace_filename.dbf' SIZE 100M AUTOEXTEND ON;

--2. TIMED_STATISTICS 파라미터 활성화
ALTER SYSTEM SET TIMED_STATISTICS=TRUE;

--3. STATSPACK 설치 (PERFSTAT 유저 생성 및 STAT$ 테이블 생성)
sqlplus / as sysdba
SQL> @?/rdbms/admin/spcreate.sql
Enter value for perfstat_password:    --PW 입력
Enter value for default_tablespace:   --PERFSTAT 전용 테이블스페이스 지정
Enter value for temporary_tablespace: --PERFSTAT 전용 Temporary 테이블스페이스 지정
```

<br>

### 사용
```sql
--1. PERFSTAT 접속
sqlplus perfstat/pw

--2. 스냅샷 생성
SQL> EXEC STATSPACK.SNAP;

--3. 스냅샷 확인
SQL> SELECT SNAP_ID, SNAP_TIME FROM STATS$SNAPSHOT;
/*
SNAP_ID SNAP_TIME
------- ---------
...     ...
5       15-AUG-24
6       16-AUG-24
*/

--4. 레포트 생성 (레포트를 생성하려면 스냅샷이 최소 2개 이상 필요함)
SQL> @?/rdbms/admin/spreport.sql
/*
Listing all Completed Snapshots
 
                               Snap
Instance     DB Name        Snap Id   Snap Started    Level Comment
------------ ------------ --------- ----------------- ----- --------------------
ORCL19       ORCL19       5         28 Jul 2020 23:21      5
                  2 28 Jul 2020 23:23      5
 
 
 
Specify the Begin and End Snapshot Ids
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.
.
Enter value for begin_snap: 1 [스냅샷 시작점 입력(스냅샷 ID)]
Begin Snapshot Id specified: 1
 
Enter value for end_snap: 2  [스냅샷 종료점 입력(스냅샷 ID)]
End   Snapshot Id specified: 2
 
Enter value for report_name: [리포트 이름 입력, 엔터시 sp_(스냅샷 시작점)_(스냅샷 종료점).lst 으로 파일 생성됨]

--(옵션) 5. 스냅샷 삭제
SQL> @?/rdbms/admin/sppurge.sql

--(옵션) 6. Statspack 삭제
SQL> @?/rdbms/admin/spdrop.sql
```

<br>

### 여러 옵션
1. 데이터 수집 자동화 (Automatic Data Gathering)

    주기적으로 데이터를 수집할 때 `DBMS_JOB` 을 사용하는 경우라면 `JOB_QUEUE_PROCESSES` 파라미터가 1보다 큰 값으로 설정되어야 함
    ```sql
    ALTER SYSTEM SET JOB_QUEUE_PROCESSES = 10;

    --잡에 등록 (PERFSTAT으로만 실행되며 Interval은 기본 1시간)
    sqlplus perfstat/pw
    SQL> start @?/rdbms/admin/spauto.sql 
    ```

1. 스냅샷 레벨 (Snapshot Level)
    ```sql
    --스냅샷 레벨 설정
    EXEC STATSPACK.SNAP(i_snap_level => 10);
 
    -- 스냅샷 레벨의 기본값 변경 
    EXEC STATSPACK.MODIFY_STATSPACK_PARAMETER(i_snap_level => 0);
    ```
    |레벨|설명|
    |-|-|
    |0|일반적인 성능 통계 정보 수집|
    |1||
    |5|기본값이며 LEVEL 0 + 리소스를 많이 사용하는 SQL 정보 추가|
    |6|LEVEL 5 + SQL 상세 실행 계획 정보 추가|
    |7|LEVEL 6 + 세그먼트 정보 추가|
    |10|LEVEL 7 + 부모, 자식 Latch 정보 추가|
    >당연히 LEVEL이 높아질 수록 사용하는 리소스가 높아짐

<br>

### AWS RDS Oracle Statspack
>https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.Statspack.html

<br>
