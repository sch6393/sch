Merge Into
===

### 기본 형식
```sql
--데이터를 변경하는 테이블
MERGE INTO owner.table_name ALIAS1
USING (
	--변경할 데이터를 가져오는 테이블
	TABLE, VIEW, SubQuery
) ALIAS2
--조인 조건
ON (ALIAS1.column = ALIAS2.column ...)
--조건에 맞는 데이터가 있는 경우 Update
WHEN MATCHED THEN
  UPDATE SET column = value ...
--조건에 맞는 데이터가 없을 경우 Insert
WHEN NOT MATCHED THEN
  INSERT (column1, column2, ...) VALUES (value1, value2, ...)
;
```

<br>

### 자기 자신과 비교할 경우
```sql
MERGE INTO owner.table_name
USING DUAL
ON (column1 = value1 AND column2 = value2 ...)
WHEN MATCHED THEN
  UPDATE SET column = value ...
WHEN NOT MATCHED THEN
  INSERT (column1, column2, ...) VALUES (value1, value2, ...)
;
```
