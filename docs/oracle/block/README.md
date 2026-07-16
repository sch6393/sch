Block
===

### 설명
데이터를 처리하는 최소 작업 단위

<br>

### 상세
오라클의 데이터 구조는 아래와 같음
  * OS Block ➞ Oracle Block ➞ [Extent](../extent/README.md) ➞ [Segment](../segment/README.md) ➞ [Tablespace](../tablespace/README.md) ➞ Database

그러므로 입출력을 최적화 하기 위해선 OS 블록 사이즈의 배수가 되어야함  
일반적으로 OLTP에선 블록 사이즈가 작고 OLAP에선 블록 사이즈가 큼

<br>

### Row Chaining, Migration
* Row Chaining
  * 해당 블록의 저장 공간이 부족해 다른 블록에 데이터를 이어서 저장해야하는 상황을 말함
  * 성능 저하 문제가 발생함
  * 크기를 증가 시키면 해결될 가능성이 있음 (Wait 현상이 발생하여 오히려 성능이 떨어질 가능성도 있음)

<br>

* Row Migration
  * 해당 블록의 저장 공간이 부족해 다른 블록으로 데이터가 이동한 상황을 말함
  * 기존 블록에는 이동한 주소 정보가 있음
  * 성능 저하 문제가 발생함
  * reorg 하거나 [PCT FREE](../pct/README.md#ptcfree) 값을 증가시키면 되지만 공간 낭비로 인해 성능이 떨어질 가능성도 있음

<br>

### 테이블 블록 할당 계산
```sql
--1. 통계정보 갱신
EXEC DBMS_STATS.gather_table_stats('owner_name', 'table_name');

--2. 해당 테이블 오브젝트들의 세그먼트 확인
SELECT SEGMENT_NAME, BLOCKS, BYTES / 1024 / 1024 AS "MB" 
FROM DBA_SEGMENTS 
WHERE SEGMENT_NAME IN ('object_name1','object_name2', ...);
/*
SEGMENT_NAME	BLOCKS	MB
table_name	942168	7360.6875
index_name	149616	1168.875
*/

--3. 블록 확인
SELECT TABLE_NAME, NUM_ROWS, BLOCKS
FROM DBA_TABLES 
WHERE TABLE_NAME = 'table_name';
/*
TABLE_NAME	NUM_ROWS	BLOCKS
table_name	47344213	940759
*/

--4. 실제 사용하고 있는 블록 확인
SELECT COUNT(DISTINCT DBMS_ROWID.ROWID_BLOCK_NUMBER(ROWID) || DBMS_ROWID.ROWID_RELATIVE_FNO(ROWID)) "USED" FROM owner_name.table_name;
/*
USED
934768
*/
```

<br>

### 관련 파라미터
* [db_file_multiblock_read_count](../parameter/db_file_multiblock_read_count.md)

<br>
