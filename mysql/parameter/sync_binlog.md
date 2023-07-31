sync_binlog
===
>설정한 값의 쿼리 개수 이후 디스크 동기화

### https://dev.mysql.com/doc/refman/8.0/en/replication-options-binary-log.html#sysvar_sync_binlog

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'sync_binlog';
/*
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| sync_binlog   | 1     |
+---------------+-------+
1 row in set (0.00 sec)
*/
```

<br>

### 관련 내용
* 기본적으로 Binary log와 디스크의 동기화는 MySQL이 아닌 OS에서 처리하도록 되어 있음
* 장애 발생시 (특히 OS상이나 하드웨어 장비 문제) Binary log의 마지막 일부분들이 소실될 가능성이 있음
* 이럴 때 `sync_binlog` 파라미터로 설정한 값의 쿼리 개수 이후 디스크와 동기화 하도록 설정 가능함
* 기본값은 1로 설정되어 있으나 I/O 처리 부하가 올라감
* 1로 설정해도 Binary log와 디스크가 일치하지 않을 가능성이 있음 (커밋 요청 후 Binary log를 기록하고 InnoDB에 해당 트랜잭션을 커밋하는데 이 과정 중에 장애가 발생했을 시 InnoDB는 롤백되나 Binary log는 그대로 남아 있는 현상이 발생)
* 위의 문제는 [`innodb_support_xa`](./innodb_support_xa.md) 파라미터를 1로 설정함으로서 해결 가능 ([`innodb_support_xa`](./innodb_support_xa.md) 파라미터는 `5.7.10` 이후로 Deprecated 되고 강제 활성화되었음)
* 최대한의 내구성과 일관성을 위해서 아래와 같이 사용하는 것을 권장
    ```
    sync_binlog = 1
    innodb_flush_log_at_trx_commit = 1
    ```


<br>

### 관련 파라미터
* [innodb_support_xa](./innodb_support_xa.md)
* [innodb_flush_log_at_trx_commit](./innodb_flush_log_at_trx_commit.md)

<br>
