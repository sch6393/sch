User
===

>https://dev.mysql.com/doc/refman/8.0/en/account-management-statements.html

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

### 권한
```sql
GRANT ALL PRIVILEGES ON *.* TO 'user_name'@'hostname';
```

<br>

### 삭제
```sql
DROP USER 'user_name'@'hostname';
```

<br>

### 
