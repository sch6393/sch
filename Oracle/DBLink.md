DB Link
===

### 원격 접속
* [원격 데이터베이스 접속](./SQLPlus.md#원격-데이터베이스-접속)

<br>

### 리스트 확인
```sql
SELECT * FROM ALL_DB_LINKS;

--전체
SELECT * FROM DBA_DB_LINKS;
```

<br>

### 생성
```sql
CREATE DATABASE LINK database_link_name CONNECT TO remote_user_name IDENTIFIED BY remote_password USING 'tnsname.ora_alias_name';
CREATE PUBLIC DATABASE LINK database_link_name CONNECT TO remote_user_name IDENTIFIED BY remote_password USING 'tnsname.ora_alias_name';

--바로 지정해서 생성하는 법
CREATE DATABASE LINK database_link_name CONNECT TO remote_user_name IDENTIFIED BY remote_password USING
'(description=(address=(protocol=tcp)
(host=0.0.0.0)
(port=1521))
(connect_data=(sid=sid_name)))';
```
>되도록이면 SID에 바로 지정하지 않고 tnsname.ora에 지정해서 사용, RDS의 경우 바로 지정해서 생성해야 함

<br>

### 삭제
```sql
DROP DATABASE LINK database_link_name;
DROP PUBLIC DATABASE LINK database_link_name;
```

<br>

###
