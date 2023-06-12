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
