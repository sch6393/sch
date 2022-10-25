테이블 스페이스 내부 데이터 크기를 확인하여 변경
===

### 1. 블록 크기 확인
```sql
SHOW PARAMETER db_block_size;
/*
NAME          TYPE    VALUE 
------------- ------- ----- 
db_block_size integer 8192  
*/
```

<br>

### 2. RESIZE 쿼리 실행
```sql
SELECT 
    'ALTER TABLESPACE ' || TABLESPACE_NAME || ' RESIZE ' ||
    CASE
        WHEN CEIL((NVL(HWM, 1) * db_block_size) / 1024 / 1024) <= 100
            THEN 100
        ELSE CEIL((NVL(HWM, 1) * db_block_size) / 1024 / 1024)
    END || 'M;' AS "SQL"
FROM DBA_DATA_FILES F, (
    SELECT FILE_ID, MAX(BLOCK_ID + BLOCKS - 1 ) AS "HWM" FROM DBA_EXTENTS GROUP BY FILE_ID
) E
WHERE
    F.FILE_ID = E.FILE_ID(+)
    AND CEIL(BLOCKS * db_block_size / 1024 / 1024) - CEIL((NVL(HWM, 1) * db_block_size) / 1024 / 1024) > 0
    --AND F.FILE_ID NOT IN ( ... )  --필요에 따라 시스템 테이블 스페이스 제외
ORDER BY F.FILE_ID;
/*
SQL
------------------------------------------------
ALTER TABLESPACE tablespace_name1 RESIZE 100M;
ALTER TABLESPACE tablespace_name2 RESIZE 100M;
...
*/
```

<br>

### 3. RESIZE 쿼리 실행 (db_block_size 직접 입력)
```sql
SELECT 
    'ALTER TABLESPACE ' || TABLESPACE_NAME || ' RESIZE ' ||
    CASE
        WHEN CEIL((NVL(HWM, 1) * &&db_block_size) / 1024 / 1024) <= 100
            THEN 100
        ELSE CEIL((NVL(HWM, 1) * &&db_block_size) / 1024 / 1024)
    END || 'M;' AS "SQL"
FROM DBA_DATA_FILES F, (
    SELECT FILE_ID, MAX(BLOCK_ID + BLOCKS - 1 ) AS "HWM" FROM DBA_EXTENTS GROUP BY FILE_ID
) E
WHERE
    F.FILE_ID = E.FILE_ID(+)
    AND CEIL(BLOCKS * &&db_block_size / 1024 / 1024) - CEIL((NVL(HWM, 1) * &&db_block_size) / 1024 / 1024) > 0
    --AND F.FILE_ID NOT IN ( ... )  --필요에 따라 시스템 테이블 스페이스 제외
ORDER BY F.FILE_ID;
```

<br>
