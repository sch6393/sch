Patroni
===
>https://patroni.readthedocs.io/en/latest/

### 주의점
* `pip install` 할 때
  >after connection broken by 'NewConnectionError('<pip._vendor.urllib3.connection.HTTPSConnection object at 0x7fe59954e670>: Failed to establish a new connection: [Errno 101] Network is unreachable')'
 
  이라는 에러 메시지가 표시될 경우 네트워크 설정을 확인할 것. (VPN이나 프록시 설정이 따로 있는지 등)
  ```sh
  # 프록시인 경우 설정 방법
  /root/.config/pip/pip.conf
  #[global]
  #proxy = xxx.xxx.xxx.xxx:xxxx

  # 설정 명령어
  pip config set global.proxy xxx.xxx.xxx.xxx:xxxx
  ```

* `pip install` 할 때
  >WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv

  이라는 메시지가 표시될 경우 유저별 환경이 다른지 확인. root로 실행할 경우 Package Manager에서 Permission 문제가 발생할 가능성이 있다는 의미.
  >https://github.com/pypa/pip/issues/1668

<br>

### 전제조건
* PostgreSQL과 ETCD가 설치되어 있어야 함.
* __ETCD가 정상적으로 작동되어야 함.__
* __PostgreSQL이 초기화되어 있지 않아야 함.__

<br>

### 설치
1. 필요한 환경변수 설정
    ```sh
    NAMESPACE=postgres
    SCOPE=pg-cluster
    IP_ADDRESS1=xxx.xxx.xxx.xxx     # HOSTNAME으로 해도 문제 없음
    IP_ADDRESS2=xxx.xxx.xxx.xxx     # HOSTNAME으로 해도 문제 없음
    IP_ADDRESS3=xxx.xxx.xxx.xxx     # HOSTNAME으로 해도 문제 없음
    IP_ADDRESS_CIDR=xxx.xxx.xxx.xxx/xx
    PW_POSTGRES=postgres
    PW_REPLICATOR=replicator
    PW_PGREWIND=pgrewind
    ```

1. `pip` 와 관련 Package 설치
    ```sh
    dnf install python3-pip
    dnf install python3-psycopg2
    dnf install python-devel

    # dnf 설치 후
    pip install wheel
    pip install psycopg2-binary
    pip install python-etcd
    pip install patroni[etcd,consul]
    ```

1. `patroni` 의 설치확인
    ```sh
    patroni --version
    #patroni 4.0.4
    ```

<br>

### 설정
1. 설정파일 작성 (노드 1번일 경우의 예시)
    ```yml
    mkdir /etc/patroni
    vi /etc/patroni/patroni.yml

    namespace: ${NAMESPACE}
    scope: ${SCOPE}
    name: `hostname`

    restapi:
        listen: ${IP_ADDRESS1}:8008
        connect_address: ${IP_ADDRESS1}:8008

    etcd3:
        host: ${IP_ADDRESS1}:2379

    bootstrap:
    # this section will be written into Etcd:/<namespace>/<scope>/config after initializing new cluster
    dcs:
        ttl: 30
        loop_wait: 10
        retry_timeout: 10
        maximum_lag_on_failover: 1048576

        postgresql:
            use_pg_rewind: true
            use_slots: true
            parameters:
                #archive_command: pgbackrest --stanza=${SCOPE} archive-push /var/pgsql/data/%p
                archive_mode: true
                archive_timeout: 60s
                checkpoint_completion_target: 0.9
                effective_cache_size: 12GB
                effective_io_concurrency: 200
                fsync: true
                hot_standby: true
                log_autovacuum_min_duration: 0
                log_checkpoints: 'on'
                log_connections: 'on'
                log_disconnections: 'on'
                log_filename: 'postgresql-%Y-%m-%d_%H.log'
                log_line_prefix: '%t [%p - %l]: error=%e, %qdb=%d, user=%u, remote=%r, app=%a, client=%h, '
                log_lock_waits: 'on'
                log_min_duration_statement: 0
                log_rotation_age: 60
                log_rotation_size: 0
                log_temp_files: 0
                logging_collector: 'on'
                maintenance_work_mem: 1GB
                max_connections: 150
                max_replication_slots: 10
                max_wal_senders: 5
                max_wal_size: 8GB
                max_worker_processes: 8
                min_wal_size: 1GB
                random_page_cost: 1.1
                shared_buffers: 4GB
                shared_preload_libraries: 'pg_stat_statements'
                temp_buffers: 4MB
                timezone: 'Asia/Tokyo'
                wal_buffers: 16MB
                wal_keep_size: 2048MB
                wal_level: 'logical'
                wal_log_hints: true
                work_mem: 16MB
            #recovery_conf:
                #recovery_target_timeline: latest
                #restore_command: 'pgbackrest --stanza=${SCOPE} archive-get %f /var/pgsql/data/%p'

    # some desired options for 'initdb'
    initdb: # Note: It needs to be a list (some options need values, others are switches)
        - encoding: UTF8
        - data-checksums
        - lc-locale: C.utf8
        - lc-ctype: C.utf8
        - lc-messages: C.utf8
        - lc-monetary: C.utf8
        - lc-numeric: C.utf8
        - lc-time: C.utf8

    pg_hba: # Add following lines to pg_hba.conf after running 'initdb'
        - host replication replicator 127.0.0.1/32 md5
        - host replication replicator ::1/128 md5
        - host replication replicator ${IP_ADDRESS1}/32 md5
        - host replication replicator ${IP_ADDRESS2}/32 md5
        - host replication replicator ${IP_ADDRESS3}/32 md5
        - host all all ${IP_ADDRESS_CIDR} md5

    # Some additional users which needs to be created after initializing new cluster
    users:
        admin:
            password: admin
            options:
                - createrole
                - createdb

    postgresql:
        #listen: ${IP_ADDRESS1}:5432
        listen: '*'
        connect_address: ${IP_ADDRESS1}:5432
        data_dir: /var/pgsql/data
        bin_dir: /usr/local/pgsql/bin
        pgpass: /tmp/pgpass
        authentication:
            replication:
                username: replicator
                password: ${PW_REPLICATOR}
            superuser:
                username: postgres
                password: ${PW_POSTGRES}
            rewind:
                username: pgrewind
                password: ${PW_PGREWIND}

    tags:
        nofailover: false
        noloadbalance: false
        clonefrom: false
        nosync: true
    ```
    >경로나 파라미터 등 상세 부분은 변경해서 사용할 것

1. 파일 OWNER 변경
    ```sh
    chown -R postgres:postgres /etc/patroni
    ```

<br>

### 서비스 설정
1. SERVICE 파일 작성
    ```sh
    vi /usr/lib/systemd/system/patroni.service

    [Unit]
    Description=patroni
    Documentation=https://patroni.readthedocs.io/en/latest/index.html
    After=syslog.target network.target etcd.target
    Wants=network-online.target

    [Service]
    Type=simple
    User=postgres
    Group=postgres
    PermissionsStartOnly=true
    ExecStart=/usr/local/bin/patroni /etc/patroni/patroni.yml
    ExecReload=/bin/kill -HUP $MAINPID
    LimitNOFILE=65536
    KillMode=process
    KillSignal=SIGINT
    Restart=no
    TimeoutSec=30s

    [Install]
    WantedBy=multi-user.target
    ```

1. SERVICE 설정 및 시작
    ```sh
    systemctl daemon-reload
    systemctl start patroni
    systemctl enable patroni
    systemctl status patroni
    ```

1. SERVICE 로그 확인
    ```sh
    journalctl -u patroni
    ```

<br>

### 상태 확인 및 사용법
1. 상태 확인
    ```sh
    patronictl -c /etc/patroni/patroni.yml list
    #+ Cluster: cluster-name (*******************) ----+----+-----------+
    #| Member  | Host            | Role    | State     | TL | Lag in MB |
    #+---------+-----------------+---------+-----------+----+-----------+
    #| member1 | xxx.xxx.xxx.xxx | Replica | streaming | 11 |         0 |
    #| member2 | xxx.xxx.xxx.xxx | Leader  | running   | 11 |           |
    #| member3 | xxx.xxx.xxx.xxx | Replica | streaming | 11 |         0 |
    #+---------+-----------------+---------+-----------+----+-----------+
    ```

1. 강제 FAILOVER
    ```sh
    patronictl -c /etc/patroni/patroni.yml failover
    #Candidate ['member1', 'member3'] []: member1
    #Are you sure you want to failover cluster cluster-name, demoting current member2? [y/N]: y
    ```

<br>

### MEMBER 추가
1. __「추가할 서버」__ 먼저 [ETCD의 멤버를 추가](../../../etcd/README.md#멤버-추가) 할 필요가 있음.

1. __「추가할 서버」__ PATRONI 설정 파일 작성
    ```yml
    namespace: ${NAMESPACE}
    scope: ${SCOPE}
    name: `hostname`

    restapi:
        listen: ${IP_ADDRESS-추가}:8008
        connect_address: ${IP_ADDRESS-추가}:8008

    etcd3:
        host: ${IP_ADDRESS-추가}:2379

    bootstrap:
    # this section will be written into Etcd:/<namespace>/<scope>/config after initializing new cluster
    dcs:
        ttl: 30
        loop_wait: 10
        retry_timeout: 10
        maximum_lag_on_failover: 1048576

        postgresql:
            use_pg_rewind: true
            use_slots: true
            parameters:
                archive_command: pgbackrest --stanza=${SCOPE} archive-push /var/pgsql/data/%p
                archive_mode: true
                archive_timeout: 60s
                checkpoint_completion_target: 0.9
                effective_cache_size: 12GB
                effective_io_concurrency: 200
                fsync: true
                hot_standby: true
                log_autovacuum_min_duration: 0
                log_checkpoints: 'on'
                log_connections: 'on'
                log_disconnections: 'on'
                log_filename: 'postgresql-%Y-%m-%d_%H.log'
                log_line_prefix: '%t [%p - %l]: error=%e, %qdb=%d, user=%u, remote=%r, app=%a, client=%h, '
                log_lock_waits: 'on'
                log_min_duration_statement: 0
                log_rotation_age: 60
                log_rotation_size: 0
                log_temp_files: 0
                logging_collector: 'on'
                maintenance_work_mem: 1GB
                max_connections: 150
                max_replication_slots: 10
                max_wal_senders: 5
                max_wal_size: 8GB
                max_worker_processes: 8
                min_wal_size: 1GB
                random_page_cost: 1.1
                shared_buffers: 4GB
                shared_preload_libraries: 'pg_stat_statements'
                temp_buffers: 4MB
                timezone: 'Asia/Tokyo'
                wal_buffers: 16MB
                wal_keep_size: 2048MB
                wal_level: 'logical'
                wal_log_hints: true
                work_mem: 16MB
            recovery_conf:
                recovery_target_timeline: latest
                restore_command: 'pgbackrest --stanza=${SCOPE} archive-get %f /var/pgsql/data/%p'

    # some desired options for 'initdb'
    initdb: # Note: It needs to be a list (some options need values, others are switches)
        - encoding: UTF8
        - data-checksums
        - lc-locale: C.utf8
        - lc-ctype: C.utf8
        - lc-messages: C.utf8
        - lc-monetary: C.utf8
        - lc-numeric: C.utf8
        - lc-time: C.utf8

    pg_hba: # Add following lines to pg_hba.conf after running 'initdb'
        - host replication replicator 127.0.0.1/32 md5
        - host replication replicator ::1/128 md5
        - host replication replicator ${IP_ADDRESS1}/32 md5
        - host replication replicator ${IP_ADDRESS2}/32 md5
        - host replication replicator ${IP_ADDRESS3}/32 md5
        - host replication replicator ${IP_ADDRESS-추가}/32 md5
        - host all all ${IP_ADDRESS_CIDR} md5

    # Some additional users which needs to be created after initializing new cluster
    users:
        admin:
            password: admin
            options:
                - createrole
                - createdb

    postgresql:
        #listen: ${IP_ADDRESS-추가}:5432
        listen: '*'
        connect_address: ${IP_ADDRESS-추가}:5432
        data_dir: /var/pgsql/data
        bin_dir: /usr/local/pgsql/bin
        pgpass: /tmp/pgpass
        authentication:
            replication:
                username: replicator
                password: ${PW_REPLICATOR}
            superuser:
                username: postgres
                password: ${PW_POSTGRES}
            rewind:
                username: pgrewind
                password: ${PW_PGREWIND}

    tags:
        nofailover: false
        noloadbalance: false
        clonefrom: false
        nosync: true
    ```

1. __「추가할 서버」__ pg_hba.conf에 추가
    ```
    host replication replicator <host-patroni-추가>/32 md5
    ```

1. __「다른 서버」__ pg_hba.conf에 추가
    ```
    host replication replicator <host-patroni-추가>/32 md5
    ```

1. __「다른 서버」__ patroni reload
    ```sh
    patronictl -c /etc/patroni/patroni.yml reload <cluster-name>
    ```

1. __「추가할 서버」__ PATRONI 시작

1. __「HAPROXY 서버」__ HAPROXY 설정 파일에 추가
    ```
    # PRIMARY
    server node-추가 <host-patroni-추가>:5432 maxconn 150 check port 8008

    # STANDBY
    server node-추가 <host-patroni-추가>:5432 maxconn 150 check port 8008
    ```

<br>

### MEMBER 삭제
1. __「삭제할 서버」__ PATRONI 중지

1. __「다른 서버」__ PATRONI RELOAD
    ```sh
    patronictl -c /etc/patroni/patroni.yml reload <cluster-name>
    ```

1. __「HAPROXY 서버」__ HAPROXY 설정 파일로부터 해당 노드의 내용을 삭제

<br>

### POSTGRESQL의 설정 변경
1. 아래의 명령어를 실행해 변경
    ```sh
    patronictl -c /etc/patroni/patroni.yml edit-config
    ```

1. RELOAD가 필요한 파라미터는 RELOAD를 실행
    ```sh
    patronictl -c /etc/patroni/patroni.yml reload <cluster-name>
    ```

<br>

### CLUSTER 삭제
```sh
patronictl -c /etc/patroni/patroni.yml remove <cluster-name>
```

<br>

### 참고 링크
* https://docs.microfocus.com/doc/SMAX/24.4/HASQLPatroni
* https://docs.percona.com/postgresql/13/solutions/high-availability.html
