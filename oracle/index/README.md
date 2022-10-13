Index
===

### 생성
```sql
CREATE INDEX owner_name.index_name ON owner_name.table_name(column_name1, column_name2, column_name3 ...) TABLESPACE tablespace_name;
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
WHERE    TABLE_OWNER = 'owner_name' AND TABLE_NAME = 'table_name'
ORDER BY INDEX_NAME, COLUMN_POSITION;
```

<br>

### 참고
* [PCT](../pct/README.md)
* 인덱스의 컬럼 이름이 `SYS_NC...$` 의 형태일 경우
  >컬럼을 DESC (역순) 으로 생성할 경우 해당 컬럼 이름으로 지정됨
    ```sql
    --DESC로 생성된 컬럼 이름 확인
    SELECT INDEX_NAME, COLUMN_EXPRESSION, COLUMN_POSITION
    FROM   DBA_IND_EXPRESSIONS
    WHERE  TABLE_OWNER = 'owner_name' AND TABLE_NAME = 'table_name';
    ```

<br>
