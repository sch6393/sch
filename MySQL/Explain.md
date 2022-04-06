Explain
===

>[https://dev.mysql.com/doc/refman/8.0/en/explain.html](https://dev.mysql.com/doc/refman/8.0/en/explain.html)

### 기본 형식
```sql
EXPLAIN query;
/* Example
id, select_type, table, type, possible_keys, key, key_len, ref, rows, Extra
1	SIMPLE	b	ref	b_pk,b_ix1	b_pk	4	const	8	Using where; Using temporary; Using filesort
1	SIMPLE	a	ref	a_pk	a_pk	4	const	216720	Using where
*/
```

<br>

### 내용 설명
* id : SELECT를 구분하는 번호
* table : 참조하는 테이블
* select_type : SELECT 타입 구분
  * SIMPLE : UNION, 서브 쿼리가 없는 단순 SELECT
  * PRIMARY : UNION, 서브 쿼리인 경우의 첫번째 쿼리
  * UNION : UNION 쿼리에서 PRIMARY를 제외한 나머지
  * DEPENDENT_UNION : UNION과 동일하지만 외부 쿼리의 영향을 받음
  * UNION_RESULT : UNION 쿼리의 결과
  * SUBQUERY : 서브 쿼리나 서브 쿼리를 구성하는 여러 쿼리 중 첫번쨰 쿼리
  * DEPENDENT_SUBQUERY : 서브 쿼리와 동일하지만 외부 쿼리의 영향을 받음
  * DERIVED : SELECT로 추출된 테이블 (FROM 에서의 서브 쿼리 또는 인라인 뷰)
  * UNCACHEABLE SUBQUERY : 서브 쿼리와 동일하지만 외부 쿼리의 모든 값에 서브 쿼리를 다시 처리
  * UNCACHEABLE UNION : UNION과 동일하지만 외부 쿼리의 모든 값에 UNION 쿼리를 다시 처리
* type : 조인, 조회 타입
  * system : 테이블에 데이터가 1개 밖에 없음
  * const : Primary Key나 Unique Key로 조인이 아닌 가장 외부 테이블에 접근함 (결과는 1개)
  * eq_ref : Primary Key로 조인 (조인의 내부 테이블에 접근)
  * ref : 인덱스로 조인 (여러 행에 접근 가능)
  * ref_or_null : ref와 동일하지만 NULL도 같이 포함하여 검색
  * index_merge : 여러 개의 인덱스를 사용
  * unique_subquery : IN 안에 있는 서브 쿼리에서 Primary Key나 Unique Key를 사용
  * index_subquery : unique_subquery와 비슷하나 Primary Key나 Unique Key가 아닌 인덱스를 사용
  * range : 인덱스를 특정 범위 내에서 사용
  * index : 인덱스 풀스캔
  * all : 테이블 풀스캔
* possible_keys : 사용 가능한 인덱스 목록
* key : 옵티마이저가 실제로 사용할 인덱스
* key_len : 실제로 사용할 인덱스의 길이
* ref : 인덱스에서 값을 찾기 위해 비교할 컬럼 (상수)
* rows : 쿼리 실행 시 접근할 행 (예상 값이므로 일치하지 않을 수 있음)
* extra : 옵티마이저 동작 정보
  * Using where : 행을 가져온 후 추가적으로 검색조건을 적용해 행의 범위를 축소 (타입이 all, index라면 좋지 않음)
  * Using index : 테이블에는 접근하지 않고 인덱스로만 접근 (커버링 인덱스)
  * Using index for group-by : Using index와 비슷하지만 GROUP BY가 포함되어 있는 쿼리를 커버링 인덱스로 접근이 가능
  * Using filesort : 데이터 정렬이 필요 (filesort 사용 (MySQL의 quick sort), 데이터가 많다면 성능에 영향)
  * Using temporary : 내부적으로 임시 테이블이 생성
  * Using where with pushed : 엔진 컨디션 pushdown 최적화 (NDB만 해당)
  * Using index condition : 인덱스 컨디션 pushdown 최적화 (ICP, 멀티 칼럼 인덱스에서 왼쪽부터 순서대로 칼럼을 지정하지 않아도 인덱스를 사용)
  * Using MRR : 멀티 레인지 리드 최적화 (MRR)
  * Using join buffer(Block Nested Loop) : 조인 버퍼를 사용
  * Using join buffer(Batched Key Access) : Batched Key Access 알고리즘 (BKAJ)을 위한 조인 버퍼를 사용

<br>

### 참조
* select_type의 UNCACHEABLE SUBQUERY는 캐시된 결과를 사용할 수 없으므로 주의
* type의 const는 범위 검색으로 지정한다면 const가 아님
* type의 unique_subquery는 오버헤드가 감소함
* extra의 Using filesort는 메모리, 디스크 상의 정렬도 모두 포함
* type에 all, index가 있다면 좋지 않음
* extra에 index가 있다면 괜찮음
* 5.0 버전 이후부터는 index_merge 최적화

<br>

### JSON 형식
```sql
EXPLAIN FORMAT = JSON query;
/* Example
{
   "query_block": {
     "select_id": 1,
     "grouping_operation": {
       "using_temporary_table": true,
       "using_filesort": true,
       "nested_loop": [
         {
           "table": {
             "table_name": "b",
             "access_type": "ref",
             "possible_keys": [
               "b_pk",
               "b_ix1"
             ],
             "key": "b_pk",
             "used_key_parts": [
               "evtcode"
             ],
             "key_length": "4",
             "ref": [
               "const"
             ],
             "rows": 8,
             "filtered": 100,
             "attached_condition": "((`testdb`.`b`.`evtcode` <=> 170))"
           }
         },
         {
           "table": {
             "table_name": "a",
             "access_type": "ref",
             "possible_keys": [
               "a_pk"
             ],
             "key": "a_pk",
             "used_key_parts": [
               "evtcode"
             ],
             "key_length": "4",
             "ref": [
               "const"
             ],
             "rows": 216720,
             "filtered": 100,
             "attached_condition": "(`testdb`.`a`.`bosslife` = `testdb`.`b`.`life`)"
           }
         }
       ]
     }
   }
 }
*/


```

<br>

### 