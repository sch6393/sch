HAProxy
===
>https://www.haproxy.org/

### 설치
1. `haproxy`設置
    ```sh
    dnf install haproxy
    ```

1. 확인
    ```sh
    haproxy -v
    #HAProxy version 2.4.22-f8e3218 2023/02/14 - https://haproxy.org/
    #Status: long-term supported branch - will stop receiving fixes around Q2 2026.
    #Known bugs: http://www.haproxy.org/bugs/bugs-2.4.22.html
    #Running on Linux 5.14.0-427.26.1.el9_4.x86_64 #1 SMP PREEMPT_DYNAMIC Fri Jul 5 11:34:54 EDT 2024 x86_64
    ```

<br>

### 설정 (PostgreSQL HA 구성 예시)
1. 환경변수 설정
    ```sh
    NODE_NAME1=pg-node1
    NODE_NAME2=pg-node2
    NODE_NAME3=pg-node3
    IP_ADDRESS1=xxx.xxx.xxx.xxx # HOSTNAME으로 해도 문제 없음
    IP_ADDRESS2=xxx.xxx.xxx.xxx # HOSTNAME으로 해도 문제 없음
    IP_ADDRESS3=xxx.xxx.xxx.xxx # HOSTNAME으로 해도 문제 없음
    ```

1. 원본 설정 파일 백업
    ```sh
    cp -p /etc/haproxy/haproxy.cfg /etc/haproxy/haproxy.cfg.bkp
    ```

1. 설정파일 작성
    ```sh
    vi /etc/haproxy/haproxy.cfg

    global
        log         127.0.0.1:514 local2
        chroot      /var/lib/haproxy
        pidfile     /var/run/haproxy.pid
        maxconn     300
        user        haproxy
        group       haproxy
        daemon

        # turn on stats unix socket
        stats socket /var/lib/haproxy/stats

    defaults
        mode                    tcp
        log                     global
        retries                 5
        timeout queue           1m
        timeout connect         10s
        timeout client          10m
        timeout server          10m
        timeout check           10s
        maxconn                 300
       
    listen stats
        mode http
        bind *:7000
        stats enable
        stats uri /

    listen primary
        bind *:5000
        option httpchk /primary
        http-check expect status 200
        default-server inter 3s fall 3 rise 2 on-marked-down shutdown-sessions
        server ${NODE_NAME1} ${IP_ADDRESS1}:5432 maxconn 150 check port 8008
        server ${NODE_NAME2} ${IP_ADDRESS2}:5432 maxconn 150 check port 8008
        server ${NODE_NAME3} ${IP_ADDRESS3}:5432 maxconn 150 check port 8008

    listen replica
        bind *:6000
        option httpchk /replica
        http-check expect status 200
        #balance roundrobin
        default-server inter 3s fall 3 rise 2 on-marked-down shutdown-sessions
        server ${NODE_NAME1} ${IP_ADDRESS1}:5432 maxconn 150 check port 8008
        server ${NODE_NAME2} ${IP_ADDRESS2}:5432 maxconn 150 check port 8008
        server ${NODE_NAME3} ${IP_ADDRESS3}:5432 maxconn 150 check port 8008
    ```

<br>

### 서비스 설정
1. deamon reload
    ```sh
    systemctl daemon-reload
    ```

1. `haproxy_connect` 설정 (SELINUX가 활성화 되어 있을 경우)
    ```sh
    getsebool -a | grep haproxy_connect
    #haproxy_connect_any --> off

    # SET
    setsebool -P haproxy_connect_any=1

    # CHECK
    getsebool -a | grep haproxy_connect
    #haproxy_connect_any --> on
    ```

1. SERVICE 재시작 및 활성화
    ```sh
    systemctl start haproxy
    systemctl status haproxy
    systemctl enable haproxy
    ```

<br>

### haproxy 동작 확인
```sh
# RW (PORT : 5000)
psql -h <host-haproxy> -U postgres -p 5000 -c "SELECT pg_is_in_recovery();"
# pg_is_in_recovery
#-------------------
# f
#(1 row)

# RO (PORT : 6000)
psql -h <host-haproxy> -U postgres -p 6000 -c "SELECT pg_is_in_recovery();"
# pg_is_in_recovery
#-------------------
# t
#(1 row)
```

<br>

### `rsyslog`
1. `rsyslog` 설치
    ```
    dnf install rsyslog
    ```

1. 설정파일 작성
    ```sh
    vi /etc/rsyslog.conf

    ...
    # Provides UDP syslog reception
    # for parameters see http://www.rsyslog.com/doc/imudp.html
    module(load="imudp") # needs to be done just once             # 주석해제
    input(type="imudp" port="514")                                # 주석해제

    #### RULES ####

    # Save boot messages also to boot.log
    local7.*                                                /var/log/boot.log
    local2.*                                                /var/log/haproxy.log # 추가
    ...
    ```

1. SERVICE 재시작 및 활성화
    ```sh
    systemctl start rsyslog
    systemctl status rsyslog
    systemctl enable rsyslog
    ```

<br>
