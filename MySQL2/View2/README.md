View
===

### 생성
```sql
--기본
CREATE VIEW schema_name.table_name AS SELECT column_name1, column_name2 ... FROM schema_name.table_name;

--컬럼끼리 계산
CREATE VIEW schema_name.table_name AS
SELECT column_name1, SUM(column_name2 * column_name3) AS alias_name
FROM schema_name.table_name
GROUP BY column_name1;
```