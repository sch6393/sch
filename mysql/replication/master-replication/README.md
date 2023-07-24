Replication
===

### 주의점
1. 리플리케이션을 개시할 마스터 포지션을 알기 위해선 덤프할 때 당시의 포지션을 기록하고 있어야함
2. 덤프하면서 해당 바이너리 파일과 포지션을 남기고 싶을 때 `--master-data` 옵션을 사용하면 덤프 파일 윗부분에 이하와 같이 기록됨
    ```sql
    --
    -- Position to start replication or point-in-time recovery from
    --

    CHANGE MASTER TO MASTER_LOG_FILE='binary_log.000000', MASTER_LOG_POS=000000000;
    ```

<br>

### 설정
```sql
STOP SLAVE;
CHANGE MASTER TO MASTER_HOST='0.0.0.0', MASTER_PORT=3306, MASTER_USER='replication_user', MASTER_PASSWORD='', MASTER_LOG_FILE='binary_log.000000', MASTER_LOG_POS=000000000;
START SLAVE;

--8.0.23 이후
STOP REPLICA;
CHANGE REPLICATION SOURCE TO MASTER_HOST='0.0.0.0', MASTER_PORT=3306, MASTER_USER='replication_user', MASTER_PASSWORD='', MASTER_LOG_FILE='binary_log.000000', MASTER_LOG_POS=000000000;
START REPLICA;
```
>[GTID](../../gtid/README.md) 를 설정한 경우는 `MASTER_LOG_FILE`, `MASTER_LOG_POS` 옵션을 넣으면 안됨<br>`MASTER_AUTO_POSITION` 파라미터를 활성화 해줘야함

<br>

### 상태 확인
```sql
SHOW SLAVE STATUS\G;
/*
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 0.0.0.0
                  Master_User: replication_user
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: binary_log.000000
          Read_Master_Log_Pos: 000000000
               Relay_Log_File: relay_log.000000
                Relay_Log_Pos: 000000000
        Relay_Master_Log_File: binary_log.000000
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB:
          Replicate_Ignore_DB:
           Replicate_Do_Table:
       Replicate_Ignore_Table:
      Replicate_Wild_Do_Table:
  Replicate_Wild_Ignore_Table:
                   Last_Errno: 0
                   Last_Error:
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 000000000
              Relay_Log_Space: 000000000
              Until_Condition: None
               Until_Log_File:
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File:
           Master_SSL_CA_Path:
              Master_SSL_Cert:
            Master_SSL_Cipher:
               Master_SSL_Key:
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error:
               Last_SQL_Errno: 0
               Last_SQL_Error:
  Replicate_Ignore_Server_Ids:
             Master_Server_Id: 000
                  Master_UUID: a4768922-30db-11ec-b173-0a51b4359b95
             Master_Info_File: /mysql/data/master.info
                    SQL_Delay: 0
          SQL_Remaining_Delay: NULL
      Slave_SQL_Running_State: Slave has read all relay log; waiting for the slave I/O thread to update it
           Master_Retry_Count: 000
                  Master_Bind:
      Last_IO_Error_Timestamp:
     Last_SQL_Error_Timestamp:
               Master_SSL_Crl:
           Master_SSL_Crlpath:
           Retrieved_Gtid_Set:
            Executed_Gtid_Set:
                Auto_Position: 0
1 row in set (0.00 sec)

ERROR:
No query specified
*/

--마스터에서 현재 바이너리 로그와 포지션 확인
SHOW MASTER STATUS;
/*
+-------------------+-----------+--------------+------------------+-------------------+
| File              | Position  | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+-------------------+-----------+--------------+------------------+-------------------+
| binary_log.000000 | 000000000 |              |                  |                   |
+-------------------+-----------+--------------+------------------+-------------------+
1 row in set (0.00 sec)
*/
```

<br>

### 참고
* 순서
  1. 마스터에서 binary log를 만들어 이벤트를 기록
  1. 슬레이브의 I/O 스레드를 통해 마스터한테 이벤트를 요청
  1. 마스터는 이벤트를 요청 받으면 binlog dump 스레드를 통해 클라이언트로 이벤트를 전송
  1. 슬레이브의 I/O 스레드는 전송 받은 이벤트를 relay log로 작성
  1. SQL 스레드는 relay log를 읽고 이벤트를 실행해 슬레이브에 데이터 작성

|항목|설명|
|-|-|
|[binary-log](../../log/binary/README.md)|해당 문서 참조|
|binlog dump Thread|마스터에 저장된 binary log를 슬레이브로 전송해주는 스레드<br>마스터는 슬레이브에 대한 정보를 가지고 있지 않기 때문에 슬레이브에서 요청이 오면 그에 해당하는 이벤트만 보낼 뿐이므로 부하가 적음|
|I/O Thread|마스터한테 다음 이벤트를 요청하는 스레드|
|[relay-log](../../log/relay/README.md)|해당 문서 참조|
|SQL Thread|I/O 스레드가 만든 relay log를 읽어 실행하고, relay log를 삭제하는 스레드|

* [GTID](../../gtid/README.md)

<br>
