innodb_flush_sync
===
>체크포인트에서 발생하는 I/O 작업이 버스트될 때 [innodb_io_capacity](./innodb_io_capacity.md) 파라미터 무시 여부 설정

### https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_flush_sync

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_flush_sync';
/*
+-------------------+-------+
| Variable_name     | Value |
+-------------------+-------+
| innodb_flush_sync | OFF   |
+-------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
* https://dev.mysql.com/doc/refman/8.0/en/innodb-configuring-io-capacity.html

|설정값|설정값 (숫자)|설명|
|-|-|-|
|`ON`|`1`|[innodb_io_capacity](./innodb_io_capacity.md) 파라미터 값을 준수함|
|`OFF`|`0`|[innodb_io_capacity](./innodb_io_capacity.md) 파라미터 값을 무시함|

<br>

### 관련 파라미터
* [innodb_io_capacity](./innodb_io_capacity.md)
* [innodb_io_capacity_max](./innodb_io_capacity_max.md)

<br>
