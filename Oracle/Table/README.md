Table
===

### Table 생성
```sql
CREATE TABLE owner_name.table_name (
   column_name1  data_type  constraint_name
  ,column_name2  data_type  constraint_name
  ...
  ,column_name8  data_type  constraint_name
  ,column_name9  data_type  constraint_name
  ,CONSTRAINT "pk_name" PRIMARY KEY (column_name)
  --,CONSTRAINT "pk_name" PRIMARY KEY (column_name1, column_name2, ... )
) TABLESPACE tablespace_name;
```

<br>

### Table 이름 변경
```sql
ALTER TABLE owner_name.table_name RENAME TO change_name;
```
>자주 발생하는 에러 : [ORA-14047: ALTER TABLE|INDEX RENAME May Not Be Combined With Other Operations](./Error/14047.md), [ORA-00972: identifier is too long](./Error/00972.md)

<br>

### Column 변경
```sql
--추가
ALTER TABLE owner_name.table_name ADD column_name data_type;

--삭제
ALTER TABLE owner_name.table_name DROP COLUMN column_name;

--수정
ALTER TABLE owner_name.table_name MODIFY column_name data_type;
```

<br>

### Remap
```sql
ALTER TABLE owner_name.table_name MOVE TABLESPACE tablespace_name;
```

<br>

### BIN$... Table
```sql
--Recyclebin 테이블 조회
SHOW RECYCLEBIN;

--Recyclebin 테이블 복원
FLASHBACK TABLE table_name TO BEFORE DROP;

--Recyclebin 테이블 전체 제거
PURGE RECYCLEBIN;

--Recyclebin 특정 테이블만 제거
PURGE TABLE table_name;

--DBA 권한으로 Recyclebin 테이블 전체 제거
PURGE DBA_RECYCLEBIN;
```
>10g 이후 버전부터 DROP 테이블 할 경우 Recyclebin 이라는 곳으로 보내진다

<br>
