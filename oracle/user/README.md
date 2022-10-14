User
===

### 조회
```sql
--User
SELECT * FROM ALL_USERS;

--DBA
SELECT * FROM DBA_USERS;
```

<br>

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

### 잠금
```sql
ALTER USER user_name ACCOUNT LOCK;
ALTER USER user_name ACCOUNT UNLOCK;
```

<br>

### 비밀번호 만료 명령
```sql
ALTER USER user_name PASSWORD EXPIRE;
```

<br>

### 삭제
```sql
DROP USER user_name CASCADE;
```
>CASCADE가 없으면 [ORA-01922](../error/01922.md)가 발생함

<br>

### 기본 테이블스페이스 변경
```sql
ALTER USER user_name DEFAULT TABLESPACE tablespace_name;
```

<br>

### 비밀번호 대소문자 구분 여부
```sql
--확인
SHOW PARAMETER sec_case;
/*
TRUE  : 구분 O
FALSE : 구분 X
*/

--변경
ALTER SYSTEM SET sec_case_sensitive_logon=FALSE;
```

<br>
