User
===

### 확인
```sql
--Shell
\du

--Query
SELECT * FROM PG_USER;
/*
 usename  | usesysid | usecreatedb | usesuper | userepl | usebypassrls |  passwd  | valuntil | useconfig
----------+----------+-------------+----------+---------+--------------+----------+----------+-----------
 postgres |       10 | t           | t        | t       | t            | ******** |          |
(1 rows)
*/

SELECT * FROM PG_SHADOW;
```

<br>

### 생성
```sql
CREATE USER user_name PASSWORD 'password' role_name;
/*
CREATE ROLE
*/
```

<br>

### 삭제
```sql
DROP USER user_name;
/*
DROP ROLE
*/
```

<br>

### 비밀번호 변경
```sql
ALTER USER user_name WITH PASSWORD 'new_password';
/*
ALTER ROLE
*/
```

<br>

### 권한 부여
>https://www.postgresql.org/docs/current/sql-createrole.html
```sql
CREATE ROLE user_name role_name;
```

<br>

### 슈퍼유저 권한 부여
```sql
ALTER ROLE user_name WITH SUPERUSER;

# RDS PostgreSQL
GRANT rds_superuser TO user_name;
```

<br>

### 권한 해제
```sql
DROP ROLE user_name;
```

<br>
