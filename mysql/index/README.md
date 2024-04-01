Index
===

### MySQL의 인덱스는 [B-Tree 구조](../../etc/btree/README.md)

<br>

### 생성
```sql
--CREATE
CREATE INDEX index_name ON schema_name.table_name(column_name1, column_name2, column_name3 ...);

--ALTER
ALTER TABLE schema_name.table_name ADD INDEX index_name(column_name1, column_name2, column_name3 ...);
```

<br>

### 이름 변경
```sql
ALTER TABLE schema_name.table_name RENAME INDEX index_name_old TO index_name_new;
```

<br>

### 확인
```sql
SHOW INDEX FROM schema_name.table_name;
```

<br>

### 삭제
```sql
DROP INDEX schema_name.index_name ON schema_name.table_name;
```

<br>
