innodb_flush_log_at_timeout
===
>디스크 플러쉬 빈도 설정

### https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_flush_log_at_timeout

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_flush_log_at_timeout';
/*
+-----------------------------+-------+
| Variable_name               | Value |
+-----------------------------+-------+
| innodb_flush_log_at_timeout | 1     |
+-----------------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
* 단위는 초 단위

<br>

### 관련 파라미터
* [innodb_flush_log_at_trx_commit](./innodb_flush_log_at_trx_commit.md)

<br>
