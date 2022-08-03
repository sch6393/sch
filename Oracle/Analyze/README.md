Analyze
===

### 목적
1. Serial Statistics Gathering
1. 옵티마이저가 가장 효율적인 실행 계획을 수립하기 위한 계산에 사용
1. 각 오브젝트의 구조 및 체인 생성 여부를 확인함으로서 시스템 관리에 사용

<br>

### 생성하는 정보
|오브젝트|항목|
|-|-|
|테이블|전체 레코드 수<br>전체 Block 수<br>Block에서 사용 가능한 빈 공간의 평균<br>체인이 발생된 레코드 수<br>레코드 평균 길이|
|인덱스|인덱스 깊이 (Depth)<br>Leaf Block 수<br>Distinct Key 수<br>Leaf Blocks/Key의 평균<br>Data Blocks/Key의 평균<br>Clustering Factor<br>가장 큰 Key 값<br>가장 작은 Key 값|
|컬럼|Distinct 수<br>히스토그램 정보|
|클러스터|각 클러스터 Key 길이 평균|

<br>

### 사용
```sql
ANALYZE object_type owner_name.object_name operation_name STATISTICS;

--특정 컬럼
ANALYZE TABLE owner_name.table_name operation_name STATISTICS FOR COLUMNS column_name;

--유효성 검사
ANALYZE object_type owner_name.object_name VALIDATE STRUCTURE;
```

<br>

### 옵션에 들어갈 값
|옵션 표기|값|설명|
|-|-|-|
|object_type|TABLE|테이블 이름|
|object_type|INDEX|인덱스 이름|
|object_type|CLUSTER|클러스터 이름|
|operation_name|COMPUTE|통계 정보를 정확하게 계산함|
|operation_name|ESTIMATE|딕셔너리 값과 데이터 견본을 가지고 검사하여 통계 정보를 추정함|
|operation_name|DELETE|통계 정보를 삭제|

<br>

### 참고
1. 정기적인 ANALYZE가 필요
    1. 테이블 재생성 및 새로 클러스터링한 경우
    1. 인덱스 추가 및 재생성한 경우
    1. 대량의 데이터를 처리한 경우
1. `USER_TABLES`, `USER_COLUMNS`, `USER_INDEXS`, `USER_CLUSTER` 등의 딕셔너리 관련된 항목들도 ANALYZE를 해주면 좋음
1. 2만 레코드를 기준으로 작다면 `COMPUTE`, 많다면 `ESTIMATE` 사용을 권장
1. [DBMS_STATS과의 차이](./Analyze_DBMS_STAT.md)
1. [ORA-38029: object statistics are locked](../Error/38029.md) 에러가 발생할 경우

<br>

### SQL
```sql
--테이블 간단 통계 정보 조회
SELECT OWNER, TABLE_NAME, NUM_ROWS, CHAIN_CNT, BLOCKS, EMPTY_BLOCKS, AVG_SPACE, AVG_ROW_LEN 
FROM ALL_TABLES
WHERE OWNER = 'owner_name' AND TABLE_NAME = 'table_name';

--인덱스 간단 통계 정보 조회
SELECT OWNER, INDEX_NAME, TABLE_NAME, STATUS, NUM_ROWS, LEAF_BLOCKS, BLEVEL
FROM ALL_INDEXES
WHERE OWNER = 'owner_name' AND index_name = 'index_name';

--테이블 ANALYZE SQL
SELECT
    'ANALYZE TABLE ' || owner || '.' || table_name ||
    CASE
        WHEN NUM_ROWS >= 20000
            THEN ' ESTIMATE '
        ELSE ' COMPUTE '
    END ||
    'STATISTICS;'
FROM ALL_TABLES
WHERE OWNER = 'owner_name';

--인덱스 ANALYZE SQL
SELECT
    'ANALYZE INDEX ' || owner || '.' || index_name ||
    CASE
        WHEN NUM_ROWS >= 20000
            THEN ' ESTIMATE '
        ELSE ' COMPUTE '
    END ||
    'STATISTICS;'
FROM ALL_INDEXES
WHERE OWNER = 'owner_name';
```

<br>
