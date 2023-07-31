session_cached_cursors
===
>세션마다 닫힌 캐시 커서의 최대 수 설정

### 확인
```sql
SHOW PARAMETER session_cached_cursors;
/*
NAME                   TYPE    VALUE 
---------------------- ------- ----- 
session_cached_cursors integer 50    
*/
```

<br>

### 변경
```sql
ALTER SYSTEM SET session_cached_cursors=0000;
```

<br>

### 정보
* 값이 너무 낮은 경우 [하드 파싱](../parsing/README.md#하드-파싱)이 일어날 수 있음
* 값이 너무 높은 경우 메모리 과점유가 일어나 메모리 부족 현상이 일어날 수 있음
* SGA 메모리 사용량을 주시하면서 50씩 줄이거나 늘리면서 모니터링
    ```sql
    SELECT NAME, (VALUE / 1024 / 1024) AS "MB" FROM V$SGA;
    /*
    NAME	MB
    Fixed Size	12.99756622
    Variable Size	4992
    Database Buffers	42496
    Redo Buffers	115
    */
    ```

<br>
