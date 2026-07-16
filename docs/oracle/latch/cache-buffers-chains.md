Cache Buffers Chains
===
>Cache Buffers Chains Latch를 얻는 과정에서 경합이 발생해 나타나는 이벤트

### 내용
버퍼 캐시를 사용하기 위해서 해시 체인을 탐색하거나 변경하려는 프로세스는 무조건 해당 체인을 관리하는 Cache Buffers Chains Latch를 얻어야 하기 때문. 읽기 전용 목적으로 체인을 탐색하는 경우는 Shared로 공유할 수 있어 경합을 줄일 수 있음

<br>

### 원인과 진단, 해결
|원인|진단 방법|해결 방안|
|-|-|-|
|튜닝되지 않은 SQL|동시에 여러 프로세스가 넓은 범위의 인덱스, 테이블을 스캔할 경우 많이 발생할 수 있음<br>해당 이벤트가 발생할 때 `V$SQLAREA`, [TRACE](../trace/README.md) 로 어떤 SQL에 문제가 있는지 확인|SQL 튜닝|
|Hot Block|`V$LATCH_CHILDREN` 에서 Cache Buffers Chains Latch에 해당하는 특정 Child Latch의 CHILD#, GETS, SLEEPS 값이 높은지 확인<br>`V$SESSION_WAIT` 에서 Latch의 중복된 주소가 많은지 확인|[PCTFREE](../pct/README.md#ptcfree) 값을 높게 설정하거나 작은 크기의 블록 사용<br>파티셔닝<br>해당 블록의 로우 데이터에 대해서만 삭제 후 재삽입 작업 (테이블)|

* 아래의 SQL 결과로 특정 래치에 대한 편중 현상이 없다면 Hot Block에 의한 문제는 없다고 판단할 수 있음
    ```sql
    SELECT * FROM (
        SELECT ADDR, CHILD#, GETS, SLEEPS FROM V$LATCH_CHILDREN
        WHERE NAME = 'cache buffers chains'
        ORDER BY SLEEPS DESC
    ) WHERE ROWNUM <= 20;
    ```

<br>

### Hot Block
```sql
SELECT DBA_O.OWNER, BH.OBJD, DBA_O.OBJECT_NAME, COUNT(BH.BLOCK#) AS BLOCKS, DBA_T.BLOCK_SIZE, T.NAME AS TABLESPACE_NAME
FROM V$BH BH
JOIN DBA_OBJECTS DBA_O ON (DBA_O.OBJECT_ID = BH.OBJD)
JOIN V$TABLESPACE T ON (T.TS# = BH.TS#)
JOIN DBA_TABLESPACES DBA_T ON (DBA_T.TABLESPACE_NAME = T.NAME)
WHERE
    BH.STATUS <> 'free'
    --AND DBA_O.OWNER NOT IN ('SYS', ... )
GROUP BY DBA_O.OWNER, BH.OBJD, DBA_O.OBJECT_NAME, DBA_T.BLOCK_SIZE, T.NAME;
```

<br>

### 관련 내용
* [Latch](./README.md)

<br>

