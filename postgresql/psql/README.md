psql
===

### 접속
```sh
# 기본
psql -U user_name -d database_name

# 호스트, 포트 지정
psql -h hostname -p port -U user_name -d database_name
```

<br>

### 쿼리 실행
```sql
PGPASSWORD=password psql -U user_name -d database_name -c 'SELECT current_timestamp'

# 파일 안에 있는 쿼리 실행
PGPASSWORD=password psql -U user_name -d database_name -f file_name.sql
```

<br>

### 유저 확인
```sql
\du
/*
                                   List of roles
 Role name |                         Attributes                         | Member of
-----------+------------------------------------------------------------+-----------
 postgres  | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
*/
```

<br>

### 데이터베이스 리스트 확인
```sql
\l
/*
                                  List of databases
   Name    |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges
-----------+----------+----------+-------------+-------------+-----------------------
 postgres  | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
 template0 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
           |          |          |             |             | postgres=CTc/postgres
(3 rows)
*/
```

<br>
