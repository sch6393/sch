innodb_write_io_threads
===
>InnoDB의 쓰기 작업을 위한 I/O Thread 개수 설정

### https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_write_io_threads

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_write_io_threads';
/*
+-------------------------+-------+
| Variable_name           | Value |
+-------------------------+-------+
| innodb_write_io_threads | 4     |
+-------------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
* [Double Write Buffer](../double-write-buffer/README.md) 문서 참조
* 기본값은 `4`이며 보통 CPU 코어 갯수만큼 설정

<br>

### 관련 파라미터
* [innodb_doublewrite](./innodb_doublewrite.md)
* [innodb_read_io_threads](./innodb_read_io_threads.md)
* [sync_binlog](./sync_binlog.md)

<br>
