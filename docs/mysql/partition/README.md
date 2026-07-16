Partition
===
>[https://dev.mysql.com/doc/refman/8.0/en/partitioning.html](https://dev.mysql.com/doc/refman/8.0/en/partitioning.html)

### 구성
```sql
# 테이블 생성 시 정의
CREATE TABLE schema_name.table_name (
  column_name1   type_name   constraint_name,
  column_name2   type_name   constraint_name,
  ...
  column_name8   type_name   constraint_name,
  column_name9   type_name   constraint_name
)
# Range
PARTITION BY RANGE COLUMNS (column_name) (
  PARTITION partition_name1 VALUES LESS THAN ( value ),
  PARTITION partition_name2 VALUES LESS THAN ( value ),
  ...
  PARTITION partition_name8 VALUES LESS THAN ( value ),
  PARTITION partition_name9 VALUES LESS THAN ( value )
);

# List
PARTITION BY LIST (column_name) (
  PARTITION partition_name1 VALUES IN ( values ),
  PARTITION partition_name2 VALUES IN ( values ),
  ...
  PARTITION partition_name8 VALUES IN ( values ),
  PARTITION partition_name9 VALUES IN ( values )
);

# Hash
PARTITION BY HASH (column_name) # 표현식 가능
PARTITIONS 4;

# Key
PARTITION BY KEY ()
PARTITIONS 2;


# 테이블 생성 후 정의 (Range)
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
# Range
ALTER TABLE schema_name.table_name ADD PARTITION(
  PARTITION partition_name VALUES LESS THAN ( value )
);
```

<br>

### 재정의
```sql
# Range
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
# 파티션 별 삭제
ALTER TABLE schema_name.table_name DROP PARTITION partition_name;

# 파티션 전체 삭제
ALTER TABLE schema_name.table_name REMOVE PARTITIONING;
```

<br>

### 파티션 확인
```sql
SELECT * FROM INFORMATION_SCHEMA.PARTITIONS WHERE TABLE_NAME = 'table_name';

# 정보 확인
EXPLAIN SELECT * FROM INFORMATION_SCHEMA.PARTITIONS WHERE TABLE_NAME = 'table_name';
```

<br>

### 파티션 SELECT
```sql
SELECT * FROM schema_name.table_name PARTITION (partition_name);
SELECT * FROM schema_name.table_name PARTITION (partition_name1, partition_name2, ...);
```

<br>

### 파일 오픈 개수 확인
```sql
# 테이블을 파일 단위로 관리하고 있기 때문에 파티션 테이블의 경우 파티션 수 만큼 열어야 함
SHOW VARIABLES LIKE 'open_files_limit';
/*
Variable_name	Value
open_files_limit	65535
*/
```

<br>

### 주의점
1. 파티션 생성 조건
    * 파티셔닝 키가 정수 또는 정수로 해석되는 식이여야만 함
    * 2가지 예외
      1. LINEAR 파티션일 경우 `TEXT`, `BLOB` 이외의 데이터 형식을 키로 사용 가능
      1. RANGE, LIST 파티션일 경우 문자열, `DATE`, `DATETIME` 형식을 키로 사용 가능. 단 `TEXT`, `BLOB` 는 예외

1. 테이블을 파일 단위로 관리하고 있기 때문에 파티션 테이블의 경우 파티션 수 만큼 열어야 함
    ```sql
    SHOW VARIABLES LIKE 'open_files_limit';
    /*
    Variable_name	Value
    open_files_limit	65535
    */
    ```

1. 최대 파티션 수는 8192개 (하위 파티션 포함)

1. 아래의 기능들이 지원되지 않음
    * Foreign Key
    * 쿼리 캐시
    * FULLTEXT 인덱스
    * [공간 열 (Spatial Columns)](../spatial/README.md)

1. 사용자 정의 파티셔닝의 경우 생성된 시점의 [SQL 모드](../sql-mode/README.md) 를 유지하지 않으므로 테이블에 대한 함수, 연산자 결과로 인해 데이터가 손실될 수 있음. 되도록이면 SQL 모드 를 변경하지 말 것
    * 예를 들어 `NO_UNSIGNED_SUBTRACTION` 모드가 유효한 경우에 `CREATE TABLE` 을 실행할 수 있는데 해당 모드가 유효한 상태에서 테이블을 작성 후 해당 모드를 무효화 시키면 테이블에 액세스를 할 수 없게됨
      >[ERROR 1563 (HY000): Partition constant is out of partition function domain](../error/1563.md)

<br>
