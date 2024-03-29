ASMM (Automatic Shared Memory Management)
===
>https://docs.oracle.com/en/database/oracle/oracle-database/19/admin/managing-memory.html#GUID-0E0EBCD5-6134-492B-9232-3F76D92B1900

### 정보
SGA를 자동으로 관리하는 자동 메모리 관리 기법

<br>

### 설정 순서
1. `SGA_TARGET` 값 변경
    ```sql
    ALTER SYSTEM SET SGA_TARGET = XXX;

    --SGA_TARGET 값 계산
    SELECT
        (SELECT SUM(value) FROM V$SGA) - (SELECT CURRENT_SIZE FROM V$SGA_DYNAMIC_FREE_MEMORY) AS "SGA_TARGET"
    FROM DUAL;
    ```

1. `SGA_MAX_SIZE` 값 변경 (옵션)
    ```sql
    ALTER SYSTEM SET SGA_MAX_SIZE = XXX;
    ```

1. 이하의 파라미터 값 변경
    ```sql
    ALTER SYSTEM SET SHARED_POOL_SIZE = 0;
    ALTER SYSTEM SET LARGE_POOL_SIZE = 0;
    ALTER SYSTEM SET JAVA_POOL_SIZE = 0;
    ALTER SYSTEM SET DB_CACHE_SIZE = 0;
    ALTER SYSTEM SET STREAMS_POOL_SIZE = 0;
    ```

1. `MEMORY_TARGET` 값 변경
    ```sql
    ALTER SYSTEM SET MEMORY_TARGET = 0;
    ```
    >해당 값이 0이면 AMM이 비활성화됨

<br>

### `SGA_TARGET` ADVICE 값 확인
```sql
SELECT * FROM V$SGA_TARGET_ADVICE ORDER BY SGA_SIZE;
/*
SGA_SIZE, SGA_SIZE_FACTOR, ESTD_DB_TIME, ESTD_DB_TIME_FACTOR, ESTD_PHYSICAL_READS, ESTD_BUFFER_CACHE_SIZE, ESTD_SHARED_POOL_SIZE, CON_ID
824	0.5	187367	2.0304	1302706	160	560	0
1236	0.75	92327	1.0005	112696	480	672	0
1648	1	92281	1	101528	560	992	0
2060	1.25	92272	0.9999	99294	800	1120	0
2472	1.5	92272	0.9999	99203	1200	1120	0
2884	1.75	92272	0.9999	99203	1600	1120	0
3296	2	92272	0.9999	99203	2000	1120	0
*/
```

<br>

### 관련 내용
* [AMM](../automatic-memory-management/README.md)
* [HugePage](../hugepage/README.md)

<br>
