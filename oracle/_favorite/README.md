상황별 자주 쓰는 SQL
===
>`Ctrl + F`로 키워드 검색

### 테이블스페이스 정보 간단 확인
```sql
SELECT
    A.TABLESPACE_NAME                           AS "TABLESPACE NAME",
    A.FILE_NAME                                 AS "DIRECTORY",
    (A.BYTES - B.FREE)                          AS "USED",
    B.FREE                                      AS "FREE",
    A.BYTES                                     AS "TOTAL",
    TO_CHAR((B.FREE / A.BYTES * 100), '999.99') AS "FREE PERCENTAGE", 
    A.AUTOEXTENSIBLE                            AS "EXTEND"
FROM (
    SELECT FILE_ID, TABLESPACE_NAME, FILE_NAME, SUM(BYTES) AS "BYTES", AUTOEXTENSIBLE
    FROM DBA_DATA_FILES
    GROUP BY FILE_ID, TABLESPACE_NAME, FILE_NAME, AUTOEXTENSIBLE
) A, (
    SELECT TABLESPACE_NAME, FILE_ID, SUM(NVL(BYTES,0)) AS "FREE"
    FROM DBA_FREE_SPACE
    GROUP BY TABLESPACE_NAME, FILE_ID
) B
WHERE
    A.TABLESPACE_NAME = B.TABLESPACE_NAME
    AND A.FILE_ID = B.FILE_ID
ORDER BY A.TABLESPACE_NAME;
```

<br>

### 테이블 정보 간단 확인
```sql
--ALL_TABLES로 확인
SELECT
    TABLE_NAME                                       AS "TABLE NAME",
    NUM_ROWS                                         AS "ROWS",
    ROUND((NUM_ROWS * AVG_ROW_LEN / 1024 / 1024), 2) AS "MB"
FROM
    ALL_TABLES
WHERE
    OWNER IN ('owner_name1','owner_name2'...)
    AND TABLE_NAME IN ('table_name1','table_name2'...);

--DBA_SEGMENTS로 확인
SELECT
    OWNER, SEGMENT_NAME, SEGMENT_TYPE, BYTES / 1024 / 1024 AS "MB"
FROM
    DBA_SEGMENTS
WHERE
    OWNER = 'owner_name'
    AND SEGMENT_NAME = 'table_name'
    AND SEGMENT_TYPE = 'TABLE';
```

<br>

### 락 정보 간단 확인
```sql
--락이 걸린 오브젝트 확인
SELECT A.OBJECT_NAME, A.OBJECT_TYPE, V.*
FROM V$LOCKED_OBJECT V, ALL_OBJECTS A
WHERE V.OBJECT_ID = A.OBJECT_ID

--락 걸린 세션 확인
SELECT S.SID, S.SERIAL#, A.OBJECT_NAME
FROM V$SESSION S, V$LOCK L, ALL_OBJECTS A
WHERE
    S.SID = L.SID
    AND L.ID1 = A.OBJECT_ID
    AND L.TYPE = 'TM';

--해당 세션 강제 끊기
ALTER SYSTEM KILL SESSION 'SID, SERIAL#';
```

<br>