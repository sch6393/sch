DB Link
===

### 생성
```sql
CREATE DATABASE LINK database_link_name CONNECT TO remote_user_name IDENTIFIED BY remote_password USING 'tnsname.ora_alias_name';
```
>되도록이면 SID에 바로 지정하지 않고 tnsname.ora에 지정해서 사용

<br>

### 원격 접속
* [원격 데이터베이스 접속](./SQLPlus.md#원격-데이터베이스-접속)

<br>

### 리스트 확인
```sql
SELECT * FROM ALL_DB_LINKS;
```

<br>

###
