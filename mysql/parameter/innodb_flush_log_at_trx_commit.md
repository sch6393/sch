innodb_flush_log_at_trx_commit
===
>커밋 작업에 대해서 엄격한 ACID 준수와 고성능 간의 제어

### https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_flush_log_at_trx_commit

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_flush_log_at_trx_commit';
/*
+--------------------------------+-------+
| Variable_name                  | Value |
+--------------------------------+-------+
| innodb_flush_log_at_trx_commit | 1     |
+--------------------------------+-------+
1 row in set (0.00 sec)
*/
```

<br>

### 관련 내용
|설정값|설명|
|-|-|
|`1`|각 트랜잭션이 커밋될 때 로그를 기록하고 디스크로 플러쉬 (ACID를 완전히 준수|
|`0`|로그는 기록되지만 디스크로 플러쉬는 설정된 시간 (초) 에 맞춰 수행함 (로그가 플러쉬 되지 않은 트랜잭션은 충돌 시 손실될 수 있음)|
|`2`|각 트랜잭션 커밋이 되고 난 후에 로그를 기록하고 디스크로 플러쉬는 설정된 시간 (초) 에 맞춰 수행함 (로그가 플러쉬 되지 않은 트랜잭션은 충돌 시 손실될 수 있음)|
* `0` 과 `2` 로 설정된 경우 설정된 시간에 맞춰 플러쉬가 수행됨을 100% 보장하지 않음 (충돌 시 손상되는 트랜잭션의 양이 1초보다 적을수도, 많을수도 있다는 의미)
* 플러쉬 빈도를 설정하는 시간은 [`innodb_flush_log_at_timeout`](./innodb_flush_log_at_timeout.md) 파라미터에서 설정함


<br>

### 관련 파라미터
* [sync_binlog](./sync_binlog.md)
* [innodb_flush_log_at_timeout](./innodb_flush_log_at_timeout.md)

<br>
