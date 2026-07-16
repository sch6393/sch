Hint
===

### 힌트 종류
>A, B : Table Alias
* INDEX ACCESS
  |Hint Name|Description|Example|
  |-|-|-|
  |INDEX|인덱스 순차|`/*+ INDEX(A index_name) */`|
  |INDEX_ASC|인덱스 내림차|`/*+ INDEX_ASC(A index_name) */`|
  |INDEX_DESC|인덱스 오름차 (역순)|`/*+ INDEX_DESC(A index_name) */`|
  |INDEX_FFS|인덱스 Fast Full Scan|`/*+ INDEX_FFS(A index_name) */`|
  |PARALLEL_INDEX|인덱스 Parallel Scan|`/*+ PARALLEL_INDEX(A index_name) */`|
  |NOPARALLEL_INDEX|인덱스 No Parallel Scan|`/*+ NOPARALLEL_INDEX(A index_name) */`|
  |AND_EQUALS|인덱스 Merge|`/*+ AND_EQUALS(index_name1 index_name2) */`|
  |FULL|FULL Scan|`/*+ FULL(A) */`|

* JOIN ACCESS
  |Hint Name|Description|Example|
  |-|-|-|
  |USE_NL|Nested Loop|`/*+ USE_NL(A B) */`|
  |USE_MERGE|Sort Merge Loop|`/*+ USE_MERGE(A B) */`|
  |USE_HASH|Hash Loop|`/*+ USE_HASH(A B) */`|
  |HASH_AJ|Hash Anti Join|`/*+ HASH_AJ(A B) */`|
  |HASH_SJ|Hash Semi Join|`/*+ HASH_SJ(A B) */`|
  |NL_AJ|Nested Loop Anti Join|`/*+ NL_AJ(A B) */`|
  |NL_SJ|Nested Loop Semi Join|`/*+ NL_SJ(A B) */`|
  |MERGE_AJ|Sort Merge Anti Join|`/*+ MERGE_AJ(A B) */`|
  |MERGE_SJ|Sort Merge Semi Join|`/*+ MERGE_SJ(A B) */`|

* JOIN DRIVING
  |Hint Name|Description|Example|
  |-|-|-|
  |ORDERED|FROM 앞에서부터 참조|`/*+ ORDERED */`|
  |LEADING|테이블 참조 순서 명시|`/*+ LEADING(B A) */`|
  |DRIVING_SITE|해당 테이블 먼저 참조|`/*+ DRIVING_SITE(A) */`|

* ETC
  |Hint Name|Description|Example|
  |-|-|-|
  |APPEND|[Direct Load Path](../direct-path-load/README.md)|`/*+ APPEND */`|
  |PARALLEL|Parallel 수행|`/*+ PARALLEL(A n) */` (n : 코어 갯수)|
  |CACHE|데이터를 메모리 캐싱함|`/*+ CACHE */`|
  |NOCACHE|데이터를 메모리 캐싱하지 않음|`/*+ NOCACHE */`|
  |PUSH_SUBQ|서브 쿼리를 먼저 수행|`/*+ PUSH_SUBQ */`|
  |NO_PUSH_SUBQ|서브 쿼리를 먼저 수행하지 않음|`/*+ NO_PUSH_SUBQ */`|
  |MERGE|View Merge를 수행|`/*+ MERGE */`|
  |NO_MERGE|View Merge를 수행하지 않음|`/*+ NO_MERGE */`|
  |SWAP_JOIN_INPUTS|스와핑을 활성화함|`/*+ SWAP_JOIN_INPUTS(A) */`|
  |NO_SWAP_JOIN_INPUTS|스와핑을 비활성화함|`/*+ NO_SWAP_JOIN_INPUTS(A) */`|
  |UNNEST|서브 쿼리를 필터 방식으로 수행|`/*+ MERGE */`|
  |NO_UNNEST|서브 쿼리를 필터 방식으로 수행하지 않음|`/*+ NO_MERGE */`|
  |REWRITE|쿼리 Rewrite를 수행|`/*+ REWRITE */`|
  |NOREWRITE|쿼리 Rewrite를 수행하지 않음|`/*+ NOREWRITE */`|
  |USE_CONCAT|IN 조건을 Concatenation Access로 수행|`/*+ USE_CONCAT */`|
  |USE_EXPAND|IN 조건을 Concatenation Access로 수행하지 않음|`/*+ USE_EXPAND */`|

* 특징
  1. `UNNEST`는 Hash 테이블에 직접 지정한 테이블을 올릴 때 사용
  1. 그러므로 `NO_UNNEST`는 `PUSH_SUBQ`, `NO_PUSH_SUBQ`와 같이 사용하게 되고 `UNNEST`는 `SWAP_JOIN_INPUTS`, `NO_SWAP_JOIN_INPUTS`와 같이 사용하게 됨

<br>

### 인덱스 힌트
```sql
--특정 인덱스로 참조할 경우
SELECT /*+ INDEX(A index_name) */ * FROM owner_name.table_name A;

--역순 참조
SELECT /*+ INDEX_DESC(A index_name) */ * FROM owner_name.table_name A;

--여러 인덱스로 참조
SELECT /*+ INDEX(A index_name1) INDEX(B index_name2) */ *
FROM owner_name.table_name1 A, owner_name.table_name2 B;
```

<br>

### INSERT 힌트
```sql
--다이렉트 로드 힌트
INSERT /*+ APPEND PARALLEL(A 8) */ INTO owner_name.table_name1 A SELECT /*+ PARALLEL(B 8) */ * FROM owner_name.table_name2 B;
```
>다이렉트 로드에 대한 자세한 내용은 [해당 링크](../direct-path-load/README.md) 참조

<br>

### UPDATE, DELETE 힌트
```sql
UPDATE /*+ PARALLEL(A 8) */ owner_name.table_name A SET column_name = value WHERE column_name = value;

DELETE /*+ PARALLEL(A 8) */ FROM owner_name.table_name A WHERE column_name = value;
```

* UPDATE, DELETE의 Parallel 힌트에 대한 특징
  1. UPDATE와 DELETE는 파티션 테이블이 아닌 경우 Parallel이 수행되지 않음
  1. init.ora의 `row_locking = intend` 일 경우 INSERT, UPDATE, DELETE에서 Parallel이 수행되지 않음
  1. Trigger가 활성화된 테이블은 Parallel DML이 수행되지 않음. Trigger를 비활성화할 필요가 있음
  1. 해당 테이블에 대해 동일한 트랜잭션 내에서 UPDATE 되었다면 액세스할 수 없음. Parallel DML 수행 이후 반드시 커밋이 필요
  1. Distributed transaction 에서는 Parallel DML을 사용할 수 없음
  1. self-referential integrity, delete cascade 등에서는 Parallel DML을 사용할 수 없음
  1. Parallel DML은 OBJECT, LOB Column을 가진 테이블, 또는 클러스터 테이블에서는 사용할 수 없음

<br>
