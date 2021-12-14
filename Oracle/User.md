User
===

### 생성
```sql
CREATE USER user_name IDENTIFIED BY password;
```

<br>

### 비밀번호 변경
```sql
ALTER USER user_name IDENTIFIED BY password;
```

<br>

### 권한
```sql
GRANT CONNECT, RESOURCE, DBA TO user_name;

--SYSDBA 권한
GRANT SYSDBA TO user_name;

--테이블 스페이스 권한
GRANT UNLIMITED TABLESPACE TO user_name;
```

<br>

### 삭제
```sql
DROP USER user_name;
```

<br>

### 
