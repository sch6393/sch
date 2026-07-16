Schema
===

### 확인
```sql
--Shell
\dn

--Query
SELECT * FROM PG_NAMESPACE;
```

<br>

### 생성
```sql
CREATE SCHEMA schema_name;

--해당 유저에 종속
CREATE SCHEMA schema_name AUTHORIZATION user_name; 
```

<br>

### 삭제
```sql
DROP SCHEMA schema_name;
```

<br>
