Partition
===

### 조회
```sql
--데이터 조회
SELECT * FROM owner_name.table_name PARTITION (partition_name);

--파티션 테이블 조회
SELECT * FROM DBA_PART_TABLES;

--파티션 테이블의 파티션 조회
SELECT * FROM DBA_TAB_PARTITIONS;

--파티션 인덱스 조회
SELECT * FROM DBA_PART_INDEXES;

--파티션 인덱스의 파티션 조회
SELECT * FROM DBA_IND_PARTITIONS;

--각 파티션 테이블별 용량
SELECT SEGMENT_NAME, PARTITION_NAME, (SUM(BYTES) / 1024 / 1024) AS MB
FROM DBA_SEGMENTS
WHERE OWNER = 'owner_name' AND SEGMENT_NAME = 'table_name'
GROUP BY SEGMENT_NAME, PARTITION_NAME
ORDER BY PARTITION_NAME;
```

<br>

### 파티션 테이블 구성
```sql
--Range
CREATE TABLE owner_name.table_name (
  column_name1   type_name   constraint_name,
  column_name2   type_name   constraint_name,
  ...
  column_name8   type_name   constraint_name,
  column_name9   type_name   constraint_name
)
PARTITION BY RANGE (column_name1, column_name2, column_name3)
(
  PARTITION partition_name1 VALUES LESS THAN (value1_1, value1_2, value1_3),
  PARTITION partition_name2 VALUES LESS THAN (value2_1, value2_2, value2_3),
  ...
  PARTITION partition_name8 VALUES LESS THAN (value8_1, value8_2, value8_3),
  PARTITION partition_name9 VALUES LESS THAN (MAXVALUE)
);

--Hash
PARTITION BY HASH (column_name1, column_name2, column_name3)
(
  PARTITION partition_name1,
  PARTITION partition_name2,
  ...
  PARTITION partition_name8,
  PARTITION partition_name9 VALUES LESS THAN (MAXVALUE)
)

--Composite (서브 파티션)
PARTITION BY RANGE (column_name1, column_name2)
SUBPARTITION BY HASH (column_name3)
(
  PARTITION partition_name1 VALUES LESS THAN (value1, value2),
  PARTITION partition_name2 VALUES LESS THAN (value1, value2) (
    SUBPARTITIONS sub_partition_name1,
    SUBPARTITIONS sub_partition_name2,
    ...
    SUBPARTITIONS sub_partition_name8,
    SUBPARTITIONS sub_partition_name9 VALUES LESS THAN (MAXVALUE)
  ),
  ...
  PARTITION partition_name8 VALUES LESS THAN (value1, value2),
  PARTITION partition_name9 VALUES LESS THAN (MAXVALUE)
);

--List (단일 컬럼만 가능)
PARTITION BY LIST(column_name1)
(
  PARTITION partition_name1 VALUES(value1),
  PARTITION partition_name2 VALUES(value2),
  ...
  PARTITION partition_name8 VALUES(value8_1,value8_2,...value8_8,value8_9),
  PARTITION partition_name9 VALUES(value9),
);
```

<br>

### 파티션 테이블 기타 구성
* Reference
  ```sql
  --상속 받는 테이블의 컬럼이 없어도 해당 테이블의 파티션 키로 분할
  CREATE TABLE owner_name.table_name1
  (
    column_name1   type_name   PRIMARY KEY,
    column_name2   type_name   constraint_name,
    column_name3   type_name   constraint_name,
    ...
  )
  PARTITION BY LIST(column_name3)
  (
    PARTITION partition_name1 VALUES(value1),
    PARTITION partition_name2 VALUES(value2),
    ...
  );

  CREATE TABLE owner_name.table_name2
  (
    column_name1   type_name   PRIMARY KEY,
    column_name2   type_name   NOT NULL,
    column_name3   type_name   constraint_name,
    ...
    CONSTRAINT FK_table_name2 FOREIGN KEY (column_name2) REFERENCES table_name1
  )
  PARTITION BY REFERENCE (FK_table_name2);
  ```
  1. 해당 컬럼의 제약조건이 Foreign Key여야 함
  1. 상속 받는 테이블의 키 값이 `NOT NULL`이여야 함

<br>

* Interval
  ```sql
  --Range의 경우 미리 생성해야하고 이후 분할, 병합시 추가 작업이 필요하나 Interval의 경우는 미리 정의하기 때문에 자동으로 생성
  CREATE TABLE owner_name.table_name (
    column_name1   DATE        constraint_name,
    column_name2   type_name   constraint_name,
    ...
    column_name8   type_name   constraint_name,
    column_name9   type_name   constraint_name
  )
  PARTITION BY RANGE (column_name1) INTERVAL (NUMTOYMINTERVAL(1,'MONTH'))
  (
    PARTITION partition_name1 VALUES LESS THAN (TO_DATE('value1', 'YYYYMMDD')),
    PARTITION partition_name2 VALUES LESS THAN (TO_DATE('value2', 'YYYYMMDD')),
    ...
  );

  --특정 테이블 스페이스로 지정
  PARTITION BY RANGE (column_name1) INTERVAL (condition) STORE IN (tablespace_name1,tablespace_name2 ...)
  (
    PARTITION partition_name1 VALUES LESS THAN (value1) TABLESPACE tablespace_name1,
    PARTITION partition_name2 VALUES LESS THAN (value2) TABLESPACE tablespace_name2,
    ...
  );

  --특정 파티션에 접근
  SELECT * FROM owner_name.table_name PARTITION FOR(TO_DATE('value', 'YYYYMMDD'));
  ```
  1. 파티션 키가 `DATE`, `NUMBER` 만 가능함
  1. 도메인 인덱스를 사용하지 못함

<br>

* System
  ```sql
  --파티션을 미리 생성하는게 아닌 데이터 삽입시 파티션 지정
  CREATE TABLE owner_name.table_name (
    column_name1   type_name   constraint_name,
    column_name2   type_name   constraint_name,
    ...
    column_name8   type_name   constraint_name,
    column_name9   type_name   constraint_name
  )
  PARTITION BY SYSTEM
  (
    PARTITION partition_name1 TABLESPACE tablespace_name1,
    PARTITION partition_name2 TABLESPACE tablespace_name2,
    ...
  );

  --DML 실행 시 파티션을 지정
  INSERT INTO owner_name.table_name PARTITION(partition_name1) VALUES (value);
  DELETE FROM owner_name.table_name PARTITION(partition_name1) WHERE column_name1 = value;
  UPDATE owner_name.table_name PARTITION(partition_name1) SET column_name2 = value WHERE column_name1 = value;
  ```
  1. INSERT는 파티션 지정이 강제이므로 지정하지 않으면 [ORA-14701](../error/14701.md) 에러가 발생
  1. DELETE, UPDATE는 파티션 지정이 강제는 아니지만 지정하지 않으면 전체 파티션을 검색하게 되므로 가급적 지정

<br>

* Virtual Column
  ```sql
  --가상의 컬럼을 만든 후 해당 컬럼으로 파티션 적용
  CREATE TABLE owner_name.table_name (
    column_name1   type_name   constraint_name,
    column_name2   type_name   constraint_name,
    ...
    virtual_column_name AS (SUBSTR(column_name1,0,2)),
    ...
  )
  PARTITION BY RANGE (virtual_column_name)
  (
    PARTITION partition_name1 VALUES LESS THAN (value1),
    PARTITION partition_name2 VALUES LESS THAN (value2),
    ...
    PARTITION partition_name8 VALUES LESS THAN (value8),
    PARTITION partition_name9 VALUES LESS THAN (MAXVALUE)
  );
  ```

<br>
