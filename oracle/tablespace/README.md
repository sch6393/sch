Tablespace
===

### [BIGFILE과 SMALLFILE 차이](./bigfile_smallfile.md)
### [Temporary Tablespace](./temporary_tablespace.md)
### [UNDO Tablespace](./undo_tablespace.md)

<br>

### 확인  
```sql
SELECT * FROM DBA_DATA_FILES;

--용량별 확인
SELECT
	A.TABLESPACE_NAME,
	ROUND(CAPACITY / 1024 / 1024, 2)  AS "CAPACITY (MB)", 
	ROUND(FREE / 1024 / 1024, 2)      AS "FREE (MB)", 
	ROUND((FREE / CAPACITY * 100), 2) AS "FREE (%)" 
FROM (
	SELECT TABLESPACE_NAME, SUM(BYTES) AS CAPACITY 
	FROM   DBA_DATA_FILES GROUP BY TABLESPACE_NAME
) A
INNER JOIN (
	SELECT TABLESPACE_NAME, SUM(BYTES) as FREE 
	FROM   DBA_FREE_SPACE GROUP BY TABLESPACE_NAME
) B ON A.TABLESPACE_NAME = B.TABLESPACE_NAME
ORDER BY "FREE (MB)" DESC;
```

<br>

### 생성
```sql
--자동 확장
CREATE TABLESPACE tablespace_name DATAFILE '/AAA/BBB/CCC/tablespace_filename' SIZE size AUTOEXTEND ON NEXT size MAXSIZE UNLIMITED;

--수동 확장
CREATE TABLESPACE tablespace_name DATAFILE '/AAA/BBB/CCC/tablespace_filename' SIZE size AUTOEXTEND OFF;
```

<br>

### 추가
```sql
--자동 확장
ALTER TABLESPACE tablespace_name ADD DATAFILE '/AAA/BBB/CCC/tablespace_filename' SIZE size AUTOEXTEND ON NEXT size MAXSIZE UNLIMITED;

--수동 확장
ALTER TABLESPACE tablespace_name ADD DATAFILE '/AAA/BBB/CCC/tablespace_filename' SIZE size AUTOEXTEND OFF;
```

<br>

### 변경
```sql
ALTER TABLESPACE tablespace_name RESIZE size;
```
>[테이블 스페이스 내부 데이터 크기를 확인하여 변경](./change-tablespace.md)

<br>

### Remap
* [Table](../Table/README.md#remap)
* [Index](../Index/README.md#remap)

<br>

### 삭제
```sql
DROP TABLESPACE tablespace_name;
```

<br>

### 권한
```sql
--size만큼 사용할 수 있도록 함
ALTER USER owner_name QUOTA size ON tablespace_name;

--제한 없음
ALTER USER owner_name QUOTA UNLIMITED ON tablespace_name;

--전체 권한
GRANT UNLIMITED TABLESPACE to owner_name;
```

<br>

### RDS에서의 Tablespace
https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.CommonDBATasks.Database.html

<br>

### 구조 설명
* 데이터 할당
  |종류|설명|
  |-|-|
  |DMT|Data Dictionary에서 공간할당을 담당. 지금은 쓰지 않는 방식|
  |LMT|Extent마다 1비트를 할당해 0, 1로 공간이 있는지 없는지 표시하며 저장<br>(공간이 비어있을 수 있음)| 

<br>

* 데이터 구조
  |종류|설명|
  |-|-|
  |MSSM|Segment Header에서 프리리스트로 여유공간을 갖는 방식|
  |ASSM|Segment Header가 각 Extents의 Header들을 Point하고 있는 계층구조|

<br>

* __사용 방식__
  |종류|SQL|사용 가능 여부|
  |-|-|-|
  |LMT + ASSM (Default)|`EXTENT MANAGEMENT LOCAL SEGMENT SPACE MANAGEMENT AUTO;`|O|
  |LMT + MSSM|`EXTENT MANAGEMENT LOCAL SEGMENT SPACE MANAGEMENT MANUAL;`|O|
  |DMT + MSSM|`EXTENT MANAGEMENT DICTIONARY SEGMENT SPACE MANAGEMENT MANUAL;`|O|
  |DMT + ASSM|`EXTENT MANAGEMENT DICTIONARY SEGMENT SPACE MANAGEMENT AUTO;`|X|

<br>

* 참조
  |구분|System Tablespace|Datafile Tablespace|
  |-|-|-|
  |DBCA로 DB를 생성|LMT|LMT|
  |수동으로 DB를 생성|DMT|LMT, DMT|

<br>
