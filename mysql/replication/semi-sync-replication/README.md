Semi-sync Replication
===

### 개요
Async 방식의 Replication은 Slave의 복제 여부를 확인하지 않고 Master 에서 진행 중인 트랜잭션을 종료 및 반환함

따라서 Slave에서 복제가 여러가지 이유 (네트워크나 시스템 부하 등) 로 시점에 따른 데이터 차이가 발생되는데 이를 줄이고 데이터의 일관성을 유지하기 위함

<br>

### 설명
Master로부터 Slave에 전달된 변경 내역이 Relay log에 기록되었다는 메세지를 받고 난 후 클라이언트에 결과를 반환하는 방식

Async 방식에 비하면 성능이 조금 저하되긴 하지만 Sync 방식 (Relay log에 기록된 내용이 데이터 파일까지 적용) 방식보다는 성능 저하가 적고 일단 Slave 측에서는 변경 내역은 받았기 때문에 데이터 일관성, 안정성이 Async 방식보다 더 보장됨

<br>

### 방식
* After Commit
  ```
          ----------------Master---------------      --------Slave-------
          |User Thread             Dump Thread|      |I/O Thread        |
  Commit ----->||                      ||     |      |   ||             |
          |    ||-> Wrtie Binlog ----->||     |      |   ||             |
          |    ||-> Push ACK Request ->||     |      |   ||             |
          |    ||-> Sync Binlog        ||     |      |   ||             |
          |    ||-> Engine Commit      ||-> Send Event ->||             |
          |    ||-> Wait...            ||     |      |   ||-> Relay Log |
          |    ||                      ||<-------- ACK <-||             |
          |    ||<-- Binlog Position <-||     |      |   ||             |
  OK <---------||                      ||     |      --------------------
          |    ||                      ||     |
          -------------------------------------
  ```
  1. Commit
  1. Binlog Flush, Commit
  1. Engine Commit
  1. Binlog Dump, Send Event with ACK 요청
  1. Relay log 기록
  1. ACK 수신
  1. User Commit

<br>

* After Sync 
  ```
          ----------------Master---------------      --------Slave-------
          |User Thread             Dump Thread|      |I/O Thread        |
  Commit ----->||                      ||     |      |   ||             |
          |    ||-> Wrtie Binlog ----->||     |      |   ||             |
          |    ||-> Push ACK Request ->||     |      |   ||             |
          |    ||-> Sync Binlog        ||-> Send Event ->||             |
          |    ||-> Wait...            ||     |      |   ||-> Relay Log |
          |    ||                      ||<-------- ACK <-||             |
          |    ||<-- Binlog Position <-||     |      |   ||             |
          |    ||-> Engine Commit      ||     |      --------------------
  OK <---------||                      ||     |
          |    ||                      ||     |
          -------------------------------------
  ```
  1. Commit
  1. Binlog Flush, Commit
  1. Binlog Dump, Send Event with ACK 요청
  1. Relay log 기록
  1. ACK 수신
  1. Engine Commit
  1. User Commit

<br>

* 두 방식의 차이점
  * After Commit은 Binlog Dump를 Engine Commit 이후 전송
  * After Sync는 Binlog Dump를 Binlog Flush, Commit 이후 전송함 (Engine Commit 전)

<br>

### 설치
1. 플러그인 설치
    ```sql
    # 5.7
    INSTALL PLUGIN rpl_semi_sync_master SONAME 'semisync_master.so';
    INSTALL PLUGIN rpl_semi_sync_slave SONAME 'semisync_slave.so';

    # 8.0
    INSTALL PLUGIN rpl_semi_sync_source SONAME 'semisync_source.so';
    INSTALL PLUGIN rpl_semi_sync_replica SONAME 'semisync_replica.so';
    ```

1. 플러그인 설치 확인
    ```sql
    SELECT PLUGIN_NAME, PLUGIN_STATUS FROM INFORMATION_SCHEMA.PLUGINS WHERE PLUGIN_NAME LIKE '%semi%';

    # 5.7
    /*
    +----------------------+---------------+
    | PLUGIN_NAME          | PLUGIN_STATUS |
    +----------------------+---------------+
    | rpl_semi_sync_master | ACTIVE        |
    | rpl_semi_sync_slave  | ACTIVE        |
    +----------------------+---------------+
    */

    # 8.0
    /*
    +-----------------------+---------------+
    | PLUGIN_NAME           | PLUGIN_STATUS |
    +-----------------------+---------------+
    | rpl_semi_sync_replica | ACTIVE        |
    | rpl_semi_sync_source  | ACTIVE        |
    +-----------------------+---------------+
    */
    ```

1. 활성화 (Master, Slave 모두 다 실행 필요)
    ```sql
    # 5.7
    SET GLOBAL rpl_semi_sync_master_enabled = 1;
    SET GLOBAL rpl_semi_sync_slave_enabled = 1;

    # 8.0
    SET GLOBAL rpl_semi_sync_source_enabled = ON;
    SET GLOBAL rpl_semi_sync_replica_enabled = ON;

    # Disable : 0 / Enable : 1
    ```
    >[rpl_semi_sync_master_timeout](../../parameter/rpl_semi_sync_master_timeout.md) 파라미터로 Async 방식으로 변경되는 시간 설정 가능

1. `my.cnf` 에 등록
    ```
    # 5.7
    rpl_semi_sync_master_enabled=1
    rpl_semi_sync_slave_enabled=1
    rpl_semi_sync_master_timeout=10000

    # 8.0
    rpl_semi_sync_source_enabled=ON
    rpl_semi_sync_replica_enabled=ON
    rpl_semi_sync_source_timeout=10000
    ```

1. 상태 확인
    ```sql
    # 파라미터 설정 확인
    SHOW VARIABLES WHERE VARIABLE_NAME LIKE '%rpl_semi_sync_%';
    /*
    +-------------------------------------------+------------+
    | Variable_name                             | Value      |
    +-------------------------------------------+------------+
    | rpl_semi_sync_master_enabled              | ON         |
    | rpl_semi_sync_master_timeout              | 10000      |
    | rpl_semi_sync_master_trace_level          | 32         |
    | rpl_semi_sync_master_wait_for_slave_count | 1          |
    | rpl_semi_sync_master_wait_no_slave        | ON         |
    | rpl_semi_sync_master_wait_point           | AFTER_SYNC |
    | rpl_semi_sync_slave_enabled               | OFF        |
    | rpl_semi_sync_slave_trace_level           | 32         |
    +-------------------------------------------+------------+
    */

    # STATUS 확인
    SHOW STATUS LIKE 'Rpl_semi_sync%';

    # Master
    /*
    +--------------------------------------------+-------+
    | Variable_name                              | Value |
    +--------------------------------------------+-------+
    | Rpl_semi_sync_master_clients               | 2     |
    | Rpl_semi_sync_master_net_avg_wait_time     | 0     |
    | Rpl_semi_sync_master_net_wait_time         | 0     |
    | Rpl_semi_sync_master_net_waits             | 0     |
    | Rpl_semi_sync_master_no_times              | 0     |
    | Rpl_semi_sync_master_no_tx                 | 0     |
    | Rpl_semi_sync_master_status                | ON    |
    | Rpl_semi_sync_master_timefunc_failures     | 0     |
    | Rpl_semi_sync_master_tx_avg_wait_time      | 0     |
    | Rpl_semi_sync_master_tx_wait_time          | 0     |
    | Rpl_semi_sync_master_tx_waits              | 0     |
    | Rpl_semi_sync_master_wait_pos_backtraverse | 0     |
    | Rpl_semi_sync_master_wait_sessions         | 0     |
    | Rpl_semi_sync_master_yes_tx                | 0     |
    | Rpl_semi_sync_slave_status                 | OFF   |
    +--------------------------------------------+-------+
    */

    # Slave
    /*
    +--------------------------------------------+-------+
    | Variable_name                              | Value |
    +--------------------------------------------+-------+
    | Rpl_semi_sync_master_clients               | 0     |
    | Rpl_semi_sync_master_net_avg_wait_time     | 0     |
    | Rpl_semi_sync_master_net_wait_time         | 0     |
    | Rpl_semi_sync_master_net_waits             | 0     |
    | Rpl_semi_sync_master_no_times              | 0     |
    | Rpl_semi_sync_master_no_tx                 | 0     |
    | Rpl_semi_sync_master_status                | OFF   |
    | Rpl_semi_sync_master_timefunc_failures     | 0     |
    | Rpl_semi_sync_master_tx_avg_wait_time      | 0     |
    | Rpl_semi_sync_master_tx_wait_time          | 0     |
    | Rpl_semi_sync_master_tx_waits              | 0     |
    | Rpl_semi_sync_master_wait_pos_backtraverse | 0     |
    | Rpl_semi_sync_master_wait_sessions         | 0     |
    | Rpl_semi_sync_master_yes_tx                | 0     |
    | Rpl_semi_sync_slave_status                 | ON    |
    +--------------------------------------------+-------+
    */
    ```
    >이미 Async 복제 중인 경우 복제 중지 후 다시 시작해야 Semi-sync로 동작하게 됨. 이는 Semi-sync에서 Async도 마찬가지

<br>
