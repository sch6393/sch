Index
===

### 생성
```sql
CREATE INDEX owner_name.index_name ON owner_name.table_name(column_name1, column_name2, column_name3 ...) TABLESPACE tablespace_name;

--PK를 별도로 생성해야할 경우
ALTER TABLE owner_name.table_name ADD CONSTRAINT index_name PRIMARY KEY(column_name1, column_name2, column_name3 ...) USING INDEX TABLESPACE tablespace_name;
```

<br>

### 이름 변경
```sql
ALTER INDEX owner_name.index_name RENAME TO index_name;
```

<br>

### 삭제
```sql
DROP INDEX owner_name.index_name;
```

<br>

### Rebuild
```sql
ALTER INDEX owner_name.index_name REBUILD;
```

<br>

### Remap
```sql
ALTER INDEX owner_name.index_name REBUILD TABLESPACE tablespace_name;
```

<br>

### 테이블의 인덱스 확인
```sql
SELECT   INDEX_NAME, COLUMN_NAME, COLUMN_POSITION
FROM     ALL_IND_COLUMNS
WHERE    OWNER = 'owner_name' AND TABLE_NAME = 'table_name'
ORDER BY INDEX_NAME, COLUMN_POSITION;
```

<br>

### 참고
* [PCT](../PCT/README.md)

<br>
