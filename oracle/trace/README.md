Trace
===

### Auto Trace
```sql
--1. Auto Trace 옵션 ON
SET AUTOTRACE ON

--2. 쿼리 실행
SELECT * FROM owner_name.table_name;

--3. 자동으로 결과 출력 후 실행 계획과 실행 통계가 출력됨
Execution Plan
----------------------------------------------------------
Plan hash value: 1516826816

-------------------------------------------------------------------------------
| Id  | Operation          | Name     | Rows  | Bytes | Cost (%CPU)| Time     |
-------------------------------------------------------------------------------
|   0 | SELECT STATEMENT   |            |     1 |   135 |     2   (0)| 00:00:01 |
|*  1 |  COUNT STOPKEY     |            |       |       |            |          |
|   2 |   TABLE ACCESS FULL| table_name |     1 |   135 |     2   (0)| 00:00:01 |
-------------------------------------------------------------------------------

Predicate Information (identified by operation id):
---------------------------------------------------

   1 - filter(ROWNUM<=1)


Statistics
----------------------------------------------------------
          1  recursive calls
          0  db block gets
          3  consistent gets
          0  physical reads
          0  redo size
       2144  bytes sent via SQL*Net to client
         52  bytes received via SQL*Net from client
          2  SQL*Net roundtrips to/from client
          0  sorts (memory)
          0  sorts (disk)
          1  rows processed

--4. Auto Trace 옵션 OFF
SET AUTOTRACE OFF
```

<br>

### 실행 옵션
|명령어|설명|
|-|-|
|`SET AUTOTRACE TRACEONLY`|결과 없이 실행 계획과 실행 통계 출력|
|`SET AUTOTRACE TRACEONLY EXPLAIN`|결과 없이 실행 계획만 출력|
|`SET AUTOTRACE TRACEONLY STATISTICS`|결과 없이 실행 통계만 출력|

<br>

### Auto Trace 권한
Auto Trace를 사용하려면 `PLUSTRACE` 권한이 필요
```sql
--부여
GRANT PLUSTRACE TO owner_name;

--회수
REVOKE PLUSTRACE TO owner_name;
```

<br>

### SQL Trace
```sql
--1. SQL TRACE 활성화 (세션 레벨)
ALTER SESSION SET SQL_TRACE = TRUE;
ALTER SESSION SET EVENTS '10046 trace name context forever, level 12';

--2. 파일 구분자 설정
ALTER SESSION SET TRACEFILE_IDENTIFIER='identifier';

--3. 쿼리 실행
SELECT * FROM owner_name.table_name;

--4. SQL TRACE 비활성화
ALTER SESSION SET SQL_TRACE = FALSE;

--5. 파일 확인
sid_ora_processid_identifier.trc 

--6. 파일 변환
tkprof tracefile outputfile
```

<br>

### SQL Trace 주의점
* SQL Trace를 사용할 때 인스턴스 레벨과 세션 레벨을 선택할 수 있는데 인스턴스 레벨이면 데이터베이스 전체 퍼포먼스가 20 ~ 30% 감소하므로 세션 레벨로 생성하는 것이 좋음

<br>
