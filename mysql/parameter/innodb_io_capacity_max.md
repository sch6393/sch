innodb_io_capacity_max
===
>InnoDB 버퍼 풀에서 백그라운드 작업에 사용 가능한 최대 IOPS 설정

### https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_io_capacity_max

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_io_capacity_max';
/*
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| innodb_io_capacity_max | 3000  |
+------------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
* https://dev.mysql.com/doc/refman/8.0/en/innodb-configuring-io-capacity.html

<br>

### 관련 파라미터
* [innodb_flush_sync](./innodb_flush_sync.md)
* [innodb_io_capacity](./innodb_io_capacity.md)

<br>
