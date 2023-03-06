Partition
===
>[https://dev.mysql.com/doc/refman/8.0/en/partitioning.html](https://dev.mysql.com/doc/refman/8.0/en/partitioning.html)

### 구성
```sql
--테이블 생성 시 정의
CREATE TABLE schema_name.table_name (
  column_name1   type_name   constraint_name,
  column_name2   type_name   constraint_name,
  ...
  column_name8   type_name   constraint_name,
  column_name9   type_name   constraint_name
)
--Range
PARTITION BY RANGE COLUMNS (column_name) (
  PARTITION partition_name1 VALUES LESS THAN ( value ),
  PARTITION partition_name2 VALUES LESS THAN ( value ),
  ...
  PARTITION partition_name8 VALUES LESS THAN ( value ),
  PARTITION partition_name9 VALUES LESS THAN ( value )
);

--List
PARTITION BY LIST (column_name) (
  PARTITION partition_name1 VALUES IN ( values ),
  PARTITION partition_name2 VALUES IN ( values ),
  ...
  PARTITION partition_name8 VALUES IN ( values ),
  PARTITION partition_name9 VALUES IN ( values )
);

--Hash
PARTITION BY HASH (column_name) --표현식 가능
PARTITIONS 4;

--Key
PARTITION BY KEY ()
PARTITIONS 2;


--테이블 생성 후 정의 (Range)
ALTER TABLE schema_name.table_name PARTITION BY RANGE COLUMNS (column_name) (
  PARTITION partition_name1 VALUES LESS THAN ( value ),
  PARTITION partition_name2 VALUES LESS THAN ( value ),
  ...
  PARTITION partition_name7 VALUES LESS THAN ( value ),
  PARTITION partition_name8 VALUES LESS THAN ( value ),
  PARTITION partition_name9 VALUES LESS THAN MAXVALUE
);
```

<br>

### 추가
```sql
--Range
ALTER TABLE schema_name.table_name ADD PARTITION(
  PARTITION partition_name VALUES LESS THAN ( value )
);
```

<br>

### 재정의
```sql
--Range
ALTER TABLE schema_name.table_name REORGANIZE PARTITION partition_name INTO (
  PARTITION partition_name1 VALUES LESS THAN ( value ),
  PARTITION partition_name2 VALUES LESS THAN ( value ),
  ...
  PARTITION partition_name8 VALUES LESS THAN ( value ),
  PARTITION partition_name9 VALUES LESS THAN ( value )
);
```

<br>

### 삭제
```sql
ALTER TABLE schema_name.table_name DROP PARTITION partition_name;
```

<br>

### 파티션 확인
```sql
SELECT * FROM INFORMATION_SCHEMA.PARTITIONS WHERE TABLE_NAME = 'table_name';

--정보 확인
EXPLAIN SELECT * FROM INFORMATION_SCHEMA.PARTITIONS WHERE TABLE_NAME = 'table_name';
```

<br>

### 파일 오픈 개수 확인
```sql
--테이블을 파일 단위로 관리하고 있기 때문에 파티션 테이블의 경우 파티션 수 만큼 열어야 함
SHOW VARIABLES LIKE 'open_files_limit';
/*
Variable_name	Value
open_files_limit	65535
*/
```

<br>
