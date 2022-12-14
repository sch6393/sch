Compression
===

### 특징
1. 데이터 압축을 위해 사용
1. 일반적으로 용량을 2 ~ 3배 줄일 수 있음
1. Block을 압축 해제하지 않고도 바로 읽을 수 있기 때문에 경우에 따라 오히려 접근하는 Block수가 줄어 I/O 감소하고 버퍼 캐시를 효율적으로 사용 가능
1. UPDATE 성능이 저하됨
1. 압축 실행으로 인한 CPU 부하 및 LOCK 발생
1. Block 저장 구조가 변경되기 때문에 인덱스를 다시 빌드해줘야 함

<br>

### 테이블 압축
```sql
--Default Compression
CREATE TABLE owner_name.table_name (
    ...
) COMPRESS;

--모든 DML에서의 Compression
CREATE TABLE owner_name.table_name (
    ...
) COMPRESS FOR OLTP;

--해당 SQL 실행 이후 데이터부터 압축
ALTER TABLE owner_name.table_name COMPRESS;

--해당 SQL 실행 이후 데이터부터 압축하지 않음
ALTER TABLE owner_name.table_name NOCOMPRESS;

--Rebuild
ALTER TABLE owner_name.table_name MOVE PARTITION TABLESPACE tablespace_name COMPRESS;
```

<br>

### 인덱스 압축
```sql
--Default Compression
CREATE INDEX owner_name.index_name ON owner_name.table_name(column_name ...) COMPRESS;

--해당 SQL 실행 이후 데이터부터 압축
ALTER TABLE owner_name.index_name COMPRESS;

--해당 SQL 실행 이후 데이터부터 압축하지 않음
ALTER TABLE owner_name.index_name NOCOMPRESS;

--Rebuild
ALTER INDEX owner_name.table_name REBUILD PARTITION TABLESPACE tablespace_name COMPRESS;
```

<br>
