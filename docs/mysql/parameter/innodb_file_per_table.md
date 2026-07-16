innodb_file_per_table
===
>테이블 생성 시 어느 테이블 스페이스에서 생성할지를 설정

### https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_file_per_table

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_file_per_table';
/*
+-----------------------+-------+
| Variable_name         | Value |
+-----------------------+-------+
| innodb_file_per_table | ON    |
+-----------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
* [Tablespace](../tablespace/README.md) 문서 참조

|설정값|설명|
|-|-|
|`ON`|테이블을 파일 단위 테이블 스페이스에 생성함|
|`OFF`|테이블을 시스템 테이블 스페이스에 생성함|

<br>
