User
===

>[https://dev.mysql.com/doc/refman/8.0/en/account-management-statements.html](https://dev.mysql.com/doc/refman/8.0/en/account-management-statements.html)

<br>

### 갱신
```sql
FLUSH PRIVILEGES;
```

<br>

### 생성
```sql
CREATE USER 'user_name'@'hostname' IDENTIFIED BY 'password';
```

<br>

### 비밀번호 변경
```sql
ALTER USER 'user_name'@'hostname' IDENTIFIED BY 'password';
```

<br>

### 권한 부여, 회수
```sql
GRANT ALL PRIVILEGES ON *.* TO 'user_name'@'hostname';
REVOKE ALL PRIVILEGES ON *.* TO 'user_name'@'hostname';

--SELECT, UPDATE, INSERT, DELETE
GRANT SELECT ON *.* TO 'user_name'@'hostname';
REVOKE SELECT ON *.* FROM 'user_name'@'hostname';

--확인
SHOW GRANTS FOR 'user_name'@'hostname';
```

<br>

### SUPER 권한
```sql
GRANT SUPER ON *.* TO 'user_name'@'hostname' IDENTIFIED BY 'password';

--파라미터로 수정하는 방법
log_bin_trust_function_creators=1
```

<br>

### RDS에서 다른 admin 계정을 만드는 법
```sql
--1. admin 권한 가져오기
SHOW GRANTS for admin;
'GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER, LOAD FROM S3, SELECT INTO S3, INVOKE LAMBDA, INVOKE SAGEMAKER, INVOKE COMPREHEND ON *.* TO \'admin\'@\'%\' WITH GRANT OPTION'

--2. 유저 생성
CREATE USER 'admin2'@'%' IDENTIFIED BY 'password';

--3. 위의 권한을 가져와 생성한 유저로 변경
GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER, LOAD FROM S3, SELECT INTO S3, INVOKE LAMBDA, INVOKE SAGEMAKER, INVOKE COMPREHEND ON *.* TO 'admin2'@'%' WITH GRANT OPTION;
```

<br>

### 삭제
```sql
DROP USER 'user_name'@'hostname';
```

<br>

### 
