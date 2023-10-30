innodb_read_io_threads
===
>InnoDB의 읽기 작업을 위한 I/O Thread 개수 설정

### https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_read_io_threads

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_read_io_threads';
/*
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| innodb_read_io_threads | 4     |
+------------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
* [Read-ahead](../read-ahead/README.md) 문서 참조
* 기본값은 `4`이며 보통 CPU 코어 갯수만큼 설정
* `SHOW ENGINE INNODB STATUS` 에서 `pending read requests` 항목이 `innodb_read_io_threads` * 64 보다 크다면 `innodb_read_io_threads` 값을 늘릴 필요성이 있을 수 있음

<br>

### 관련 파라미터
* [innodb_write_io_threads](./innodb_write_io_threads.md)

<br>
