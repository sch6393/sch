View
===

### 생성
```sql
CREATE OR REPLACE VIEW owner_name.view_name AS
SELECT * FROM owner_name.table_name
;

-- Column 지정
CREATE OR REPLACE VIEW owner_name.view_name AS
SELECT
  column_name1, column_name2, column_name3, ... column_name8  ,column_name9
FROM owner_name.table_name
;

-- JOIN, FUNCTION, GROUP BY, UNION 가능
CREATE OR REPLACE VIEW owner_name.view_name AS
SELECT A.*, B.*
FROM owner_name.table_name_A A
JOIN owner_name.table_name_B B ON (A.column_name = B.column_name)
WHERE A.column_name ...
;
```

<br>

### 구조 확인
```sql
DESC owner_name.view_name;
```

<br>

### 삭제
```sql
DROP VIEW owner_name.view_name;
```

<br>
