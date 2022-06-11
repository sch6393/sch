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
GRANT CONNECT, RESOURCE TO user_name;

--프로시저 실행 권한
GRANT EXECUTE ON procedure_name TO user_name;

--DBA 권한
GRANT DBA TO user_name;

--SYSDBA 권한
GRANT SYSDBA TO user_name;

--테이블 스페이스 권한
GRANT UNLIMITED TABLESPACE TO user_name;

--가지고 있는 시스템 권한 확인
SELECT * FROM DBA_SYS_PRIVS WHERE GRANTEE = 'user_name';

--가지고 있는 롤 확인 (시스템 권한은 롤에 포함)
SELECT * FROM DBA_ROLE_PRIVS WHERE GRANTEE = 'user_name';

--가지고 있는 롤에 있는 시스템 권한 확인
SELECT * FROM DBA_SYS_PRIVS WHERE GRANTEE = 'role_name';
```

<br>

### 삭제
```sql
DROP USER user_name;
```

<br>
