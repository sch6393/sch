local_infile
===
>데이터베이스에서 외부 파일 허용 여부 설정

### https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_local_infile

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'local_infile';
/*
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| local_infile  | ON    |
+---------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
* [mysqlshell dump](../mysqlshell-dump/README.md)
* [Load Data](../load-data/README.md)
* [관련 에러 (3948)](../error/3948.md)

<br>
