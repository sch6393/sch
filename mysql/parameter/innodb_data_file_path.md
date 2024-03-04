innodb_data_file_path
>InnoDB 시스템 테이블스페이스 파일의 이름, 크기, 속성 정의

### https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_data_file_path

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_data_file_path';
/*
+-----------------------+------------------------+
| Variable_name         | Value                  |
+-----------------------+------------------------+
| innodb_data_file_path | ibdata1:12M:autoextend |
+-----------------------+------------------------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
* [시스템 테이블스페이스](../tablespace/system/README.md)
* 파일 이름, 파일 크기, 자동 확장 속성, 최대 속성을 정의함

<br>

### 관련 파라미터
* [innodb_autoextend_increment](./innodb_autoextend_increment.md)

<br>