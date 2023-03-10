binary log
===

### 포맷
|종류|특징|
|-|-|
|STATEMENT|쿼리문으로 기록하기 때문에 용량이 적고 버전에 따른 차이가 없으나 복구 및 동기화 속도가 느림<br>복구 시 데이터의 일관성 보장에 어려움이 있음 (`SYSDATE()`, `NOW()` 등)|
|ROW|변경된 데이터 기반으로 기록하기 때문에 용량이 많아지지만 복구 및 동기화 속도가 빠름<br>복구 시 데이터의 일관성 보장|
|MIXED|STATEMENT 방식과 ROW 방식을 같이 사용함<br>복구 시 데이터의 일관성 보장|
>[Isolation Level](../isolation-level/README.md) 이 `READ-COMMITTED` 일 때 STATEMENT 방식을 사용하게 되면 트랜잭션 단위로 순서대로 로깅하기 때문에 복구나 동기화 시 결과가 다를 수 있음. 그래서 `READ-COMMITTED` 에서는 ROW나 MIXED를 사용해야 함.

<br>

### 설정 확인
```sql
SHOW VARIABLES LIKE 'log_bin%';
/*
Variable_name, Value
log_bin	ON
log_bin_basename	/directory_name/log/mysql-bin-changelog
log_bin_index	/directory_name/log/mysql-bin-changelog.index
log_bin_trust_function_creators	OFF
log_bin_use_v1_row_events	OFF
*/

--binlog
SHOW VARIABLES LIKE '%binlog%';
/*
Variable_name, Value
binlog_cache_size	32768
binlog_checksum	CRC32
binlog_direct_non_transactional_updates	OFF
binlog_error_action	ABORT_SERVER
binlog_format	MIXED
binlog_group_commit_sync_delay	0
binlog_group_commit_sync_no_delay_count	0
binlog_gtid_simple_recovery	ON
binlog_max_flush_queue_time	0
binlog_order_commits	ON
binlog_row_image	FULL
binlog_rows_query_log_events	OFF
binlog_stmt_cache_size	32768
binlog_transaction_dependency_history_size	25000
binlog_transaction_dependency_tracking	COMMIT_ORDER
innodb_api_enable_binlog	OFF
innodb_locks_unsafe_for_binlog	OFF
log_statements_unsafe_for_binlog	OFF
max_binlog_cache_size	18446744073709547520
max_binlog_size	134217728
max_binlog_stmt_cache_size	18446744073709547520
sync_binlog	1
*/

--보관 기간
SHOW VARIABLES LIKE 'expire_logs_days';
/*
Variable_name, Value
expire_logs_days	7
*/
```

<br>

### 로그 파일 확인
```sql
SHOW BINARY LOGS;
/*
Log_name, File_size
binary_log.000001	536871550
binary_log.000002	536871303
binary_log.000003	536872543
...
*/
```

<br>

### Purge
```sql
--파일 기준
PURGE BINARY LOGS TO 'binary_log.000000';

--날짜 기준
PURGE BINARY LOGS BEFORE '2023-03-08 09:00:00';
```

<br>

### `sync_binlog`에 관한 내용
* 기본 값은 Binary log와 디스크의 동기화를 MySQL이 아닌 OS에서 처리하도록 되어 있음
* 장애 발생시 (특히 OS상이나 하드웨어 장비 문제) Binary log의 마지막 일부분들이 소실될 가능성이 있음
* `sync_binlog` 옵션을 설정하면 설정한 값의 쿼리 개수 이후 디스크와 동기화 하도록 설정됨
* 보통 1로 설정하나 I/O 처리 부하가 올라감
* 1로 설정해도 Binary log와 디스크가 일치하지 않을 가능성이 있음 (커밋 요청 후 Binary log를 기록하고 InnoDB에 해당 트랜잭션을 커밋하는데 이 과정 중에 장애가 발생했을 시 InnoDB는 롤백되나 Binary log는 그대로 남아 있는 현상이 발생)
* 위의 문제는 `innodb_support_xa` 옵션을 1로 설정함으로서 해결 가능 (다만 해당 옵션은 `5.7.10` 이후로 Deprecated 되었기 때문에 `5.7.10` 이전 버전에만 해당됨)
* [관련 내용](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_support_xa)

<br>
