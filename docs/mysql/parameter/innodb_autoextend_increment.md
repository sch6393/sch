innodb_autoextend_increment
===
>InnoDB 시스템 테이블스페이스가 가득 찼을 때 크기를 확장하기 위한 증분 크기 (MB)

### https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_autoextend_increment

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_autoextend_increment';
/*
+-----------------------------+-------+
| Variable_name               | Value |
+-----------------------------+-------+
| innodb_autoextend_increment | 64    |
+-----------------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
* [시스템 테이블스페이스](../tablespace/system/README.md)
* 테이블별 파일, 일반 테이블스페이스에는 해당하지 않음

<br>

### 관련 파라미터
* [innodb_data_file_path](./innodb_data_file_path.md)

<br>
