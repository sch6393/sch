relay log
===

### 설정
```sql
SHOW VARIABLES LIKE 'relay%'
/*
Variable_name, Value
max_relay_log_size	0
relay_log	/directory_name/log/relaylog
relay_log_basename	/directory_name/log/relaylog
relay_log_index	/directory_name/log/relaylog.index
relay_log_info_file	relay-log.info
relay_log_info_repository	TABLE
relay_log_purge	ON                  --1 : 삭제 (Default), 0 : 삭제 안함
relay_log_recovery	ON
relay_log_space_limit	0           --로그 용량 제한
sync_relay_log	10000
sync_relay_log_info	10000
*/
```

<br>

### Rotate 조건
1. I/O 스레드가 시작할 때
1. `FLUSH LOGS;` 쿼리가 실행될 때 (SQL 스레드가 로그를 삭제)
1. 로그 파일 사이즈가 `max_relay_log_size` 에 도달할 때 (값이 0인 경우 `max_binlog_size` 를 참조함)

<br>
