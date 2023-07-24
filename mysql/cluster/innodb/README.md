InnoDB Cluster
===
>[https://dev.mysql.com/doc/refman/8.0/en/mysql-innodb-cluster-introduction.html](https://dev.mysql.com/doc/refman/8.0/en/mysql-innodb-cluster-introduction.html)

### 구성
  1. 서버 구성
      ```
      host1  - MySQL Server, MySQL Shell
      host2  - MySQL Server, MySQL Shell
      host3  - MySQL Server, MySQL Shell
      router - MySQL Router, MySQL Shell
      ```

  1. MySQL 설치 (`host1`, `host2`. `host3`)
      >[https://dev.mysql.com/downloads/mysql/](https://dev.mysql.com/downloads/mysql/)

  1. MySQL Shell 설치 (`host1`, `host2`. `host3`, `router`)
      >[https://dev.mysql.com/downloads/shell/](https://dev.mysql.com/downloads/shell/)

  1. MySQL Router 설치 (`router`)
      >[https://dev.mysql.com/downloads/router/](https://dev.mysql.com/downloads/router/)

  1. 서비스 등록 (`host1`, `host2`. `host3`)
      ```sh
      systemctl enable mysqld
      systemctl start mysqld
      ```

  1. MySQL 초기화 (`host1`, `host2`. `host3`)
      ```sh
      # root Password
      grep 'password' /var/log/mysqld.log
      # 2023-07-14T05:57:01.546249Z 1 [Note] A temporary password is generated for root@localhost: ************

      # Initialize
      mysql_secure_installation

      # Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
      # Disallow root login remotely? (Press y|Y for Yes, any other key for No) : n
      # Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y
      # Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
      ```

  1. MySQL 재기동 (`host1`, `host2`. `host3`)
      ```sh
      service mysqld restart
      ```

  1. `admin` 유저 작성 (`host1`, `host2`. `host3`)
      ```sql
      # Connect root
      mysql -u root -p

      # Create User
      CREATE USER admin@'%' IDENTIFIED BY '**********';  
      GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' WITH GRANT OPTION;
      FLUSH PRIVILEGES;
      ```

  1. Cluster 유저 작성 (`host1`, `host2`. `host3`)
      ```sql
      mysqlsh admin@localhost -- shell store-credential "icadmin@host1:3306" "**********"
      mysqlsh admin@localhost -- shell store-credential "icadmin@host2:3306" "**********"
      mysqlsh admin@localhost -- shell store-credential "icadmin@host3:3306" "**********"
      ```

  1. 인스턴스 설정 체크 (`host1`, `host2`. `host3`)
      ```sh
      # Command
      mysqlsh -- dba configure-instance { --port=3306 --host=localhost --user=admin } --clusterAdmin=icadmin --clusterAdminPassword=********** --mycnfPath=/etc/my.cnf --interactive=true

      # Log
      Configuring local MySQL instance listening at port 3306 for use in an InnoDB cluster...

      This instance reports its own address as host1:3306
      Clients and other cluster members will communicate with it through this address by default. If this is not correct, the report_host MySQL system variable should be changed.
      Assuming full account name 'icadmin'@'%' for icadmin

      NOTE: Some configuration options need to be fixed:
      +----------------------------------+---------------+----------------+------------------------------------------------+
      | Variable                         | Current Value | Required Value | Note                                           |
      +----------------------------------+---------------+----------------+------------------------------------------------+
      | enforce_gtid_consistency         | OFF           | ON             | Update the config file and restart the server  |
      | gtid_mode                        | OFF           | ON             | Update the config file and restart the server  |
      | log_bin                          | <not present> | ON             | Update the config file and restart the server  |
      | log_slave_updates                | OFF           | ON             | Update the config file and restart the server  |
      | master_info_repository           | FILE          | TABLE          | Update the config file and restart the server  |
      | relay_log_info_repository        | FILE          | TABLE          | Update the config file and restart the server  |
      | report_port                      | <not set>     | 3306           | Update the server variable and the config file |
      | server_id                        | 0             | <unique ID>    | Update the config file and restart the server  |
      | transaction_write_set_extraction | OFF           | XXHASH64       | Update the config file and restart the server  |
      +----------------------------------+---------------+----------------+------------------------------------------------+

      Some variables need to be changed, but cannot be done dynamically on the server: an option file is required.
      Do you want to perform the required configuration changes? [y/n]: y

      Creating user icadmin@%.
      Account icadmin@% was successfully created.

      Configuring instance...
      The instance 'host1:3306' was configured to be used in an InnoDB cluster.
      NOTE: MySQL server needs to be restarted for configuration changes to take effect.

      # Restart MySQL
      service mysqld restart
      ```

  1. Cluster 생성 (`host1`)
      ```sh
      # Command
      mysqlsh icadmin@host1:3306 -- dba create-cluster TEST_CLUSTER --multiPrimary=false --memberWeight=100

      # Log
      A new InnoDB Cluster will be created on instance 'host1:3306'.

      Validating instance configuration at host1:3306...

      This instance reports its own address as host1:3306

      Instance configuration is suitable.
      NOTE: Group Replication will communicate with other members using 'host1:33061'. Use the localAddress option to override.

      * Checking connectivity and SSL configuration...

      Creating InnoDB Cluster 'TEST_CLUSTER' on 'host1:3306'...

      Adding Seed Instance...
      Cluster successfully created. Use Cluster.addInstance() to add MySQL instances.
      At least 3 instances are needed for the cluster to be able to withstand up to
      one server failure.

      <Cluster:TEST_CLUSTER>

      # 5.7인 경우 발생하는 WARNING
      WARNING: Instance 'host1:3306' cannot persist Group Replication configuration since MySQL version 5.7.43 does not support the SET PERSIST command (MySQL version >= 8.0.11 required). Please use the dba.configureLocalInstance() command locally to persist the changes.
      ```

  1. Cluster 추가 (`host1`)
      ```sh
      # Command
      mysqlsh icadmin@host1:3306 -- cluster add-instance host2:3306 --memberWeight=60
      mysqlsh icadmin@host1:3306 -- cluster add-instance host3:3306 --memberWeight=80

      # Log
      The safest and most convenient way to provision a new instance is through automatic clone provisioning, which will completely overwrite the state of 'host2:3306' with a physical snapshot from an existing cluster member. To use this method by default, set the 'recoveryMethod' option to 'clone'.

      The incremental state recovery may be safely used if you are sure all updates ever executed in the cluster were done with GTIDs enabled, there are no purged transactions and the new instance contains the same GTID set as the cluster or a subset of it. To use this method by default, set the 'recoveryMethod' option to 'incremental'.

      Incremental state recovery was selected because it seems to be safely usable.

      Validating instance configuration at host2:3306...

      This instance reports its own address as host2:3306

      Instance configuration is suitable.
      NOTE: Group Replication will communicate with other members using 'host2:33061'. Use the localAddress option to override.

      * Checking connectivity and SSL configuration...
      A new instance will be added to the InnoDB Cluster. Depending on the amount of
      data on the cluster this might take from a few seconds to several hours.

      Adding instance to the cluster...

      Monitoring recovery process of the new cluster member. Press ^C to stop monitoring and let it continue in background.
      State recovery already finished for 'host2:3306'

      The instance 'host2:3306' was successfully added to the cluster.


      # 5.7에서는 clone 모드를 지원하지 않으므로 아래와 같은 에러가 발생
      ERROR: RuntimeError: Option 'recoveryMethod=clone' not supported on target server version: '5.7.43'
      ERROR: Option 'recoveryMethod=clone' not supported on target server version: '5.7.43'

      # 5.7인 경우 발생하는 경고 (SET PERIST가 8 버전부터 지원되기 때문에 나오는 경고)
      WARNING: Instance 'host2:3306' cannot persist Group Replication configuration since MySQL version 5.7.43 does not support the SET PERSIST command (MySQL version >= 8.0.11 required). Please use the dba.configureLocalInstance() command locally to persist the changes.
      Adding instance to the cluster...

      WARNING: Instance 'host1:3306' cannot persist configuration since MySQL version 5.7.43 does not support the SET PERSIST command (MySQL version >= 8.0.11 required). Please use the dba.configureLocalInstance() command locally to persist the changes.
      The instance 'host2:3306' was successfully added to the cluster.

      # 아래의 에러가 발생하면서 클러스터 추가가 되지 않을 경우 group_replication_ip_whitelist 파라미터를 수정
      ERROR: Unable to start Group Replication for instance 'host2:3306'. Please check the MySQL server error log for more information.
      ERROR: RuntimeError: Group Replication failed to start: MySQL Error 3092 (HY000): host2:3306: The server is not configured properly to be an active member of the group. Please see more details on error log.
      ERROR: Group Replication failed to start: MySQL Error 3092 (HY000): host2:3306: The server is not configured properly to be an active member of the group. Please see more details on error log.

      # group_replication_ip_whitelist 파라미터 값 수정 (host1, host2, host3)
      # /etc/my.cnf 파일에 각 서버의 IP, IP 대역, 호스트 이름 작성 후 MySQL 재기동
      group_replication_ip_whitelist = "0.0.50.0/24,0.0.51.0/24"
      group_replication_single_primary_mode = ON
      ```

  1. Cluster 확인 (`host1`, `host2`. `host3`)
      ```sh
      # Command
      mysqlsh icadmin@localhost -- cluster status

      # Log
      {
          "clusterName": "TEST_CLUSTER",
          "defaultReplicaSet": {
              "name": "default",
              "primary": "host1:3306",
              "ssl": "REQUIRED",
              "status": "OK",
              "statusText": "Cluster is ONLINE and can tolerate up to ONE failure.",
              "topology": {
                  "host1:3306": {
                      "address": "host1:3306",
                      "memberRole": "PRIMARY",
                      "mode": "R/W",
                      "readReplicas": {},
                      "role": "HA",
                      "status": "ONLINE"
                  },
                  "host2:3306": {
                      "address": "host2:3306",
                      "memberRole": "SECONDARY",
                      "mode": "R/O",
                      "readReplicas": {},
                      "role": "HA",
                      "status": "ONLINE"
                  },
                  "host3": {
                      "address": "host3:3306",
                      "memberRole": "SECONDARY",
                      "mode": "R/O",
                      "readReplicas": {},
                      "role": "HA",
                      "status": "ONLINE"
                  }
              },
              "topologyMode": "Single-Primary"
          },
          "groupInformationSourceMember": "host1:3306"
      }
      ```

  1. 5.7일 경우 `SET PERIST` 를 사용할 수 없으므로 설정 내용을 로컬에 따로 저장해주어야 함 (`host1`, `host2`. `host3`)
      ```sh
      mysqlsh -- dba configure-local-instance { --port=3306 --host=localhost --user=admin } --clusterAdmin=icadmin --clusterAdminPassword=********** --mycnfPath=/etc/my.cnf --restart=true --interactive=true
      ```

  1. Router 설정 (`router`)
      ```sh
      # Command
      cd /aaa/bbb/ccc
      mysqlrouter --bootstrap admin@host1:3306 --user=root -d TEST

      # Log
      Please enter MySQL password for admin:
      # Bootstrapping MySQL Router instance at '/aaa/bbb/ccc/TEST'...

      Fetching Cluster Members
      trying to connect to mysql-server at host2:3306
      - Creating account(s) (only those that are needed, if any)
      - Verifying account (using it to run SQL queries that would be run by Router)
      - Storing account in keyring
      - Adjusting permissions of generated files
      - Creating configuration /aaa/bbb/ccc/TEST/mysqlrouter.conf

      Fetching Cluster Members
      trying to connect to mysql-server at host3:3306
      - Creating account(s) (only those that are needed, if any)
      - Verifying account (using it to run SQL queries that would be run by Router)
      - Storing account in keyring
      - Adjusting permissions of generated files
      - Creating configuration /aaa/bbb/ccc/TEST/mysqlrouter.conf

      # MySQL Router configured for the InnoDB Cluster 'TEST_CLUSTER'

      After this MySQL Router has been started with the generated configuration

      $ mysqlrouter -c /aaa/bbb/ccc/TEST/mysqlrouter.conf

      InnoDB Cluster 'TEST_CLUSTER' can be reached by connecting to:

      ## MySQL Classic protocol

      - Read/Write Connections: localhost:6446
      - Read/Only Connections:  localhost:6447

      ## MySQL X protocol

      - Read/Write Connections: localhost:6448
      - Read/Only Connections:  localhost:6449

      # Router 시작
      cd /aaa/bbb/ccc/TEST
      sh start.sh  # 시작
      sh stop.sh   # 정지
      ```

<br>

### 클러스터 관련 내용
* FAILOVER가 발생했을 때 나타나는 에러
  1. `WARNING: Instance is NOT a PRIMARY but super_read_only option is OFF.`
      ```sh
      # /etc/my.cnf에 해당 옵션 추가 후 재기동
      super_read_only = ON
      ```
  1. `NOTE: group_replication is stopped.`
      ```sh
      # MySQL Shell에 접속 후 해당 명령어 실행
      mysqlsh
      dba.rebootClusterFromCompleteOutage()
      ```

<br>

* 클러스터 노드 추가 및 삭제
    ```sh
    # 추가 (memberWeight는 어떤 Secondary에서 Primary가 될 것인지에 대한 가중치를 설정하는 옵션. 높을수록 우선 순위가 높음)
    mysqlsh icadmin@host1:3306 -- cluster add-instance host2:3306 --memberWeight=100

    # 삭제
    mysqlsh icadmin@host1:3306 -- cluster remove-instance host2:3306
    ```

<br>

### 라우터 관련 내용
* `[DEFAULT] dynamic_state` : InnoDB 클러스터 메타데이터 서버 주소를 추적 및 저장함. 라우터가 재기동되면 다시 로드<br>
파일명은 `state.json` 이고 해당 파일은 클러스터를 구성하는 MySQL 인스턴스 정보를 확인할 수 있음
    ```json
    {
        {
        "metadata-cache": {
            "group-replication-id": "cbe9b50c-26a5-11ee-ab5a-0a8b43dec245",
            "cluster-metadata-servers": [
                "mysql://host1:3306",
                "mysql://host2:3306",
                "mysql://host3:3306"
            ]
        },
        "version": "1.0.0"
    }
    ```

* `[metadata_cache] ttl` : 캐시되어 있는 메타 데이터에 대한 갱신 주기 설정
* `[metadata_cache] use_gr_notifications` : 그룹 복제 (Group Replication) 에 발생하는 변경 사항에 대해서 MySQL Router가 알림 받는 내용에 대한 설정<br>해당 옵션은 8.0.17 부터 추가된 기능이며 활성화되면 MySQL Router는 대부분의 클러스터 변경 사항에 대해 비동기식으로 알림을 받게 되고 MySQL Router는 그룹 복제에서 아래의 알림을 받는 다면 클러스터 메타 데이터를 갱신함
    ```
    group_replication/membership/quorum_loss
    group_replication/membership/view
    group_replication/status/role_change
    group_replication/status/state_change
    ```
    그룹 복제 알림 기능을 사용하려면 X 프로토콜 연결이 되어야 함. X 프로토콜 연결이 되지 않으면 설정된 TTL 간격으로 메타 데이터를 갱신함

* `[routing] destinations` : 애플리케이션으로부터 MySQL Router로 쿼리 요청을  받을 경우 전달해야하는 목적지 서버 정보 (또는 클러스터) 를 설정하는 옵션이며 정적 (IP, 호스트명) 과 동적 (클러스터명) 둘 다 가능함<br>`metadata-cache://cluster_name/?role`  을 통해 `PRIMARY`, `SECONDARY`, `PRIMARY_AND_SECONDARY` 선택 가능
* `[routing] route_strategy` : 특정 라우팅 방식을 설정할 수 있으며 `first-available`, `next-available`, `round-robin`, `round-robin-with-fallback` 선택 가능

<br>
