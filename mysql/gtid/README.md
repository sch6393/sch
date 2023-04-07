GTID (Global Transaction Identifier)
===
>[https://dev.mysql.com/doc/refman/8.0/en/replication-gtids.html](https://dev.mysql.com/doc/refman/8.0/en/replication-gtids.html)

### 구성
```
source_id:transaction_id
```

<br>

### `source_id` 확인
```sql
SELECT @@server_uuid;
/*
+--------------------------------------+
| @@server_uuid                        |
+--------------------------------------+
| uuid                                 |
+--------------------------------------+
*/
```

<br>

### 구성 방법
1. Parameter 설정
    * Master
        ```
        [mysqld]
        server-id=1
        log-bin=binlog
        gtid-mode=ON
        enforce-gtid-consistency=ON
        log_slave_updates=ON
        ```
    * Slave
        ```
        [mysqld]
        server-id=2
        log-bin=binlog
        gtid-mode=ON
        enforce-gtid-consistency=ON
        log_slave_updates=ON
        ```

1. 재시작 후 `gtid_mode` 확인 (Master, Slave)
    ```sql
    SHOW VARIABLES LIKE '%gtid_mode%';
    /*
    +---------------+-------+
    | Variable_name | Value |
    +---------------+-------+
    | gtid_mode     | ON    |
    +---------------+-------+
    */
    ```

1. Replication 유저 생성
    ```sql
    CREATE USER 'replication_user_name'@'%' IDENTIFIED BY 'password';
    GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'replication_user_name'@'%';
    FLUSH PRIVILEGES;
    SELECT USER, HOST FROM mysql.user;
    ```

1. [마스터로부터 덤프 파일 백업 후 슬레이브에서 복원](../mysqldump/README.md)

1. [Replication 설정 및 동작 확인](../replication/README.md)

<br>
