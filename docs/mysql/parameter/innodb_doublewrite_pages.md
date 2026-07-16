innodb_doublewrite_pages
===
>Thread 당 최대 Double Write 페이지 개수 설정

### https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_doublewrite_pages

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_doublewrite_pages';
/*
+--------------------------+-------+
| Variable_name            | Value |
+--------------------------+-------+
| innodb_doublewrite_pages | 32    |
+--------------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
* [Double Write Buffer](../double-write-buffer/README.md) 문서 참조
* 기본값으로 [`innodb_write_io_threads`](./innodb_write_io_threads.md) 만큼 설정됨

<br>

### 관련 파라미터
* [innodb_doublewrite](./innodb_doublewrite.md)
* [innodb_doublewrite_batch_size](./innodb_doublewrite_batch_size.md)
* [innodb_doublewrite_dir](./innodb_doublewrite_dir.md)
* [innodb_doublewrite_files](./innodb_doublewrite_files.md)

<br>
