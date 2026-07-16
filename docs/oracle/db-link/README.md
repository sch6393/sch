DB Link
===

### 원격 접속
* [원격 데이터베이스 접속](../sqlplus/README.md#원격-데이터베이스-접속)

<br>

### 리스트 확인
```sql
--User
SELECT * FROM ALL_DB_LINKS;

--DBA
SELECT * FROM DBA_DB_LINKS;
```

<br>

### 생성
```sql
CREATE DATABASE LINK database_link_name CONNECT TO remote_user_name IDENTIFIED BY remote_password USING 'tnsname.ora_alias_name';
CREATE PUBLIC DATABASE LINK database_link_name CONNECT TO remote_user_name IDENTIFIED BY remote_password USING 'tnsname.ora_alias_name';

--바로 지정해서 생성하는 법
CREATE DATABASE LINK database_link_name CONNECT TO remote_user_name IDENTIFIED BY remote_password USING
'(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)
(HOST=0.0.0.0)
(PORT=0000))
(CONNECT_DATA=(SID=sid_name)))';
```
>되도록이면 SID에 바로 지정하지 않고 tnsname.ora에 지정해서 사용, RDS의 경우 바로 지정해서 생성해야 함

<br>

### 권한 확인
```sql
--PUBLIC 링크 생성 권한 부여
GRANT CREATE PUBLIC DATABASE LINK TO owner_name;

--PUBLIC 링크 생성 권한 회수
GRANT DROP PUBLIC DATABASE LINK TO owner_name;

--일반 링크 생성 권한 부여
GRANT CREATE DATABASE LINK TO owner_name;
```

<br>

### 삭제
```sql
DROP DATABASE LINK database_link_name;
DROP PUBLIC DATABASE LINK database_link_name;
```

<br>
