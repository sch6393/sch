ETCD
===
>https://etcd.io/docs/v3.5/

### 전제조건
SELINUX가 비활성화여야함. 각 OS의 SELINUX 비활성화 방법에 따라 비활성화 할 것.

* https://docs.aws.amazon.com/linux/al2023/ug/disable-option-selinux.html#disable-selinux
* https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/using_selinux/changing-selinux-states-and-modes_using-selinux#Enabling_and_Disabling_SELinux-Disabling_SELinux_changing-selinux-states-and-modes

### 동작 로직
>https://raft.github.io/

Raft Consensus 알고리즘을 사용. 자세한 내용은 해당 URL을 참조.

때문에 ETCD의 최소 구성 노드 수는 3이 되고 안정적인 동작을 위해선 __3 이상의 홀수__ 여야 함

### 설치
* 해당 방법은 수동 설치를 기재 (EPEL 레포지토리가 활성화 되어 있다면 `dnf` 같은 패키지 설치가 가능하나 Amazon Linux 2023이라던지 지원하지 않는 경우는 수동 설치를 해야함

1. 환경변수지정
    ```sh
    ETCD_VER=v3.5.16
    DOWNLOAD_URL=https://github.com/etcd-io/etcd/releases/download
    ```

1. 파일 다운로드 후 압축 해제
    ```sh
    curl -L ${DOWNLOAD_URL}/${ETCD_VER}/etcd-${ETCD_VER}-linux-amd64.tar.gz -o /tmp/etcd-${ETCD_VER}-linux-amd64.tar.gz
    tar xzvf /tmp/etcd-${ETCD_VER}-linux-amd64.tar.gz -C /tmp
    ```

1. 실행파일 복사
    ```sh
    mv /tmp/etcd-${ETCD_VER}-linux-amd64/etcd /usr/bin
    mv /tmp/etcd-${ETCD_VER}-linux-amd64/etcdctl /usr/bin
    mv /tmp/etcd-${ETCD_VER}-linux-amd64/etcdutl /usr/bin
    ```

1. OWNER 변경
    ```sh
    chown root:root /usr/bin/etcd
    chown root:root /usr/bin/etcdctl
    chown root:root /usr/bin/etcdutl
    ```


1. 압축해제한 파일 삭제
    ```sh
    rm -rf /tmp/etcd-${ETCD_VER}-linux-amd64*
    ```

1. etcd 전용 유저 작성, 디렉토리 설정
    ```sh
    userdel etcd
    useradd --system --no-create-home etcd
    mkdir -p /etc/etcd /var/lib/etcd
    chown etcd:etcd /etc/etcd
    chown etcd:etcd /var/lib/etcd
    chmod 700 /var/lib/etcd
    ```

### 설정파일
1. 설정파일 작성
    ```sh
    vi /etc/etcd/etcd.conf

    # 설정값
    name: '<etcd-name>'
    data-dir: '<etcd-data-directory>'
    initial-cluster-state: 'new'
    initial-cluster-token: '<etcd-cluster-token>'
    initial-cluster: '<etcd-name-1>=http://<host-etcd-1>:2380,<etcd-name-2>=http://<host-etcd-2>:2380,<etcd-name-3>=http://<host-etcd-3>:2380'
    initial-advertise-peer-urls: 'http://<host-etcd-1>:2380'
    advertise-client-urls: 'http://<host-etcd-1>:2379'
    listen-peer-urls: 'http://<host-etcd-1>:2380'
    listen-client-urls: 'http://<host-etcd-1>:2379,http://localhost:2379'
    ```

1. 파일 OWNER 변경
    ```sh
    chown etcd:etcd /etc/etcd/etcd.conf
    ```

### 서비스 설정
1. 서비스 파일 작성
    ```sh
    vi /usr/lib/systemd/system/etcd.service

    [Unit]
    Description="etcd service"
    After=network.target

    [Service]
    LimitNOFILE=65536
    Restart=always
    Type=notify
    ExecStart=/usr/bin/etcd --config-file /etc/etcd/etcd.conf
    User=etcd
    Group=etcd

    [Install]
    WantedBy=multi-user.target
    ```

1. 서비스 설정 및 시작
    ```sh
    systemctl daemon-reload
    systemctl start etcd
    systemctl enable etcd
    systemctl status etcd
    ```

1. 서비스 로그 확인
    ```sh
    journalctl -u etcd
    ```

### 동작 확인 및 사용 방법
* 상태 확인
    ```sh
    etcdctl member list --write-out=table
    #+------------------+---------+-------+---------------------------+---------------------------+------------+
    #|        ID        | STATUS  | NAME  |        PEER ADDRS         |       CLIENT ADDRS        | IS LEARNER |
    #+------------------+---------+-------+---------------------------+---------------------------+------------+
    #| 986395a1041128fe | started | etcd1 | http://<host-etcd-1>:2380 | http://<host-etcd-1>:2379 |      false |
    #| 3b6832d933ad7704 | started | etcd2 | http://<host-etcd-2>:2380 | http://<host-etcd-2>:2379 |      false |
    #| 61e3f4d804833386 | started | etcd3 | http://<host-etcd-3>:2380 | http://<host-etcd-3>:2379 |      false |
    #+------------------+---------+-------+---------------------------+---------------------------+------------+
    ```

* 각 노드 상태 확인 (Leader 확인)
    ```sh
    etcdctl --endpoints=http://<host-etcd-1>:2380,http://<host-etcd-2>:2380,http://<host-etcd-3>:2380 endpoint status --write-out=table
    #+---------------------------+------------------+---------+---------+-----------+------------+-----------+------------+--------------------+--------+
    #|         ENDPOINT          |        ID        | VERSION | DB SIZE | IS LEADER | IS LEARNER | RAFT TERM | RAFT INDEX | RAFT APPLIED INDEX | ERRORS |
    #+---------------------------+------------------+---------+---------+-----------+------------+-----------+------------+--------------------+--------+
    #| http://<host-etcd-1>:2380 | 3b6832d933ad7704 |  3.5.16 |  918 kB |      true |      false |        19 |       1923 |   1923 |        |
    #| http://<host-etcd-2>:2380 | 61e3f4d804833386 |  3.5.16 |  913 kB |     false |      false |        19 |       1923 |   1923 |        |
    #| http://<host-etcd-3>:2380 | 986395a1041128fe |  3.5.16 |  909 kB |     false |      false |        19 |       1923 |   1923 |        |
    #+---------------------------+------------------+---------+---------+-----------+------------+-----------+------------+--------------------+--------+
    ```

* KEY 확인
    ```sh
    etcdctl get --prefix /
    ```

* KEY 추가
    ```sh
    etcdctl put test "Hello World!"
    #OK
    ```

* KEY 확인 (각 노드에 동기화가 되었는지 확인)
    ```sh
    etcdctl get test
    #test
    #Hello World!
    ```

* KEY 삭제
    ```sh
    etcdctl del test
    #1
    ```

### 멤버 추가
1. __「추가할 서버」__ etcd 설정 파일 작성
    ```sh
    vi /etc/etcd/etcd.conf

    # 설정값
    name: '<etcd-name-add>'
    data-dir: '<etcd-data-directory>'
    initial-cluster-state: 'existing'
    initial-cluster-token: '<etcd-cluster-token>'
    initial-cluster: '<etcd-name-1>=http://<host-etcd-1>:2380,<etcd-name-2>=http://<host-etcd-2>:2380,<etcd-name-3>=http://<host-etcd-3>:2380,<etcd-name-add>=http://<host-etcd-add>:2380'
    initial-advertise-peer-urls: 'http://<host-etcd-add>:2380'
    advertise-client-urls: 'http://<host-etcd-add>:2379'
    listen-peer-urls: 'http://<host-etcd-add>:2380'
    listen-client-urls: 'http://<host-etcd-add>:2379,http://localhost:2379'
    ```

1. __「Leader 서버」__ etcd 멤버 추가
    ```sh
    etcdctl member add <etcd-name-add> --peer-urls="http://<host-etcd-add>:2380"
    #Member <member-id> added to cluster <cluster-id>
    ```

1. __「추가할 서버」__ etcd 시작

1. __「그 외 서버」__ etcd 설정 파일 수정 (Leader, Member)
    ```sh
    initial-cluster-state: 'existing'
    initial-cluster: '<etcd-name-1>=http://<host-etcd-1>:2380,<etcd-name-2>=http://<host-etcd-2>:2380,<etcd-name-3>=http://<host-etcd-3>:2380,<etcd-name-add>=http://<host-etcd-add>:2380'
    ```

1. __「그 외 서버」__ (옵션) etcd 재시작

### 멤버 삭제
1. __「Leader 서버」__ MEMBER 삭제
    ```sh
    etcdctl member remove <member-id>
    #Member <member-id> removed from cluster <cluster-id>
    ```

1. __「그 외 서버」__ etcd 설정 파일 수정 (Leader, Member)
    ```sh
    #삭제할 서버 정보를 삭제
    initial-cluster: '<etcd-name-1>=http://<host-etcd-1>:2380,<etcd-name-2>=http://<host-etcd-2>:2380,<etcd-name-3>=http://<host-etcd-3>:2380'
    ```

1. __「그 외 서버」__ (옵션) etcd 재시작

### 백업
1. __「Leader 서버」__ 스냅샷 파일 작성
    ```sh
    etcdctl snapshot save /backup-directory/yyyymmdd-etcd-backup.db
    ```

1. 각 노드 설정파일 백업

### 복원
1. 스냅샷 파일 복원
    ```sh
    etcdctl snapshot restore /backup-directory/yyyymmdd-etcd-backup.db
    ```

### 참고 링크
* https://docs.redhat.com/en/documentation/openshift_container_platform/3.11/html/cluster_administration/assembly_replace-etcd-member
* https://docs.redhat.com/en/documentation/openshift_container_platform/3.11/html/cluster_administration/assembly_restore-etcd-quorum
* https://docs.microfocus.com/doc/SMAX/24.4/HASQLPatroni
* https://docs.percona.com/postgresql/13/solutions/high-availability.html
