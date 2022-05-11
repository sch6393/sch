Index
===

### 생성
```sql
CREATE INDEX owner_name.index_name ON owner_name.table_name(column_name1, column_name2, column_name3 ...) TABLESPACE tablespace_name;
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
* 
