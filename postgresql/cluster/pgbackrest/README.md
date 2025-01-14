pgbackrest
===
>https://pgbackrest.org/

>https://github.com/pgbackrest/pgbackrest

### 전제조건
* `libyaml-devel` 패키지에 의존하고 있으므로 해당 패키지가 설치되어 있어야 함.
    >https://rpmfind.net/linux/RPM/centos-stream/9/crb/x86_64/libyaml-devel-0.2.5-7.el9.x86_64.html

    >https://rpmfind.net/linux/centos-stream/9-stream/CRB/x86_64/os/Packages/libyaml-devel-0.2.5-7.el9.x86_64.rpm

<br>

### 설치
* 해당 방법은 수동 설치를 기재 (PGDG 레포지토리가 활성화 되어 있다면 `dnf` 같은 패키지 설치가 가능하나 지원하지 않는 OS거나 레포지토리 활성화가 되어 있지 않다면 수동 설치를 해야함)

1. 패키지 사전 설치
    ```sh
    dnf install -y libxml2-static
    dnf install -y libxml2-devel
    dnf install -y bzip2-devel
    dnf install -y libpq-devel

    # Amazon Linux 2023
    dnf install -y libyaml-devel

    # RHEL 9
    rpm -ivh libyaml-devel-0.2.5-7.el9.x86_64.rpm
    ```

1. 설치 파일 다운로드
    ```sh
    wget -P /tmp https://github.com/pgbackrest/pgbackrest/archive/refs/tags/release/2.54.0.tar.gz
    ```

1. 압축 해제, 설치
    ```sh
    tar xzf /tmp/2.54.0.tar.gz -C /tmp
    cd /tmp/pgbackrest-release-2.54.0/src
    ./configure && make && sudo make install
    ```

1. 확인
    ```sh
    pgbackrest --version
    # pgBackRest 2.54.0
    ```

<br>

### 설정
1. 설정파일 작성
    ```sh
    mkdir /var/log/pgbackrest
    chown -R postgres:postgres /var/log/pgbackrest

    mkdir /var/lib/pgbackrest
    chown -R postgres:postgres /var/lib/pgbackrest

    vi /etc/pgbackrest.conf

    # Configure Cluster Stanza
    [<stanza-name>]
    pg1-path=<postgresql-data-dir>
    pg1-socket-path=<postgresql-socket-dir>
    pg1-port=<postgresql-port>
    pg1-user=<postgresql-admin-user>

    # Configure the pgBackRest repository path
    [global]
    repo1-retention-full=2
    repo1-retention-diff=3
    repo1-retention-archive=2
    start-fast=y
    process-max=2
    log-level-console=info
    log-level-file=debug

    # Local
    repo1-path=/var/lib/pgbackrest

    # S3
    repo1-path=<backup-directory>
    repo1-type=s3
    repo1-s3-uri-style=path
    repo1-s3-bucket=<s3-bucket-name>
    repo1-s3-endpoint=<s3-endpoint>
    repo1-s3-key=<s3-access-key>
    repo1-s3-key-secret=<s3-secret-key>
    repo1-s3-region=<default-region-name>

    # KMS KEY
    repo1-s3-key-type=<type>
    repo1-s3-kms-key-id=<kms-key>

    [global:archive-push]
    compress-level=3
    ```

1. 설정 예시
    ```sh
    # 2개의 FULL BACKUP, 1개의 차분 BACKUP을 보존
    repo1-retention-full=2
    repo1-retention-full-type=count
    repo1-retention-diff=1

    # 2개의 FULL BACKUP을 보존하되 최신 3개의 WAL을 보존 (2개의 백업보다 이전의 백업에 대해 WAL을 삭제하지 않고 보존)
    # repo1-retention-archive-type이 incr이라면 (증분), repo1-retention-archive 옵션이 강제됨. full이라면 PITR에서 문제가 발생할 가능성이 있기 때문에 diff, incr 사용이 권장
    repo1-retention-full=2
    repo1-retention-full-type=count
    repo1-retention-archive-type=full
    repo1-retention-archive=3

    # 2개의 FULL BACKUP, 2개의 차분 BACKUP, 최신 백업 3개의 WAL을 보존 (4개의 백업 중 가장 오래된 백업에 대한 WAL은 삭제됨)
    repo1-retention-full=2
    repo1-retention-full-type=count
    repo1-retention-diff=2
    repo1-retention-archive-type=diff
    repo1-retention-archive=3

    # ↑ 백업 수로 관리하는 방침이라면 잘 확인해서 설정해야함

    # 5일 이내의 백업을 보존 (FACTOR : 일)
    repo1-retention-full=5
    repo1-retention-full-type=time
    ```

<br>

### 백업
1. STANZA 작성
    ```
    pgbackrest --stanza=<stanza-name> stanza-create
    ```

1. 체크
    ```
    pgbackrest --stanza=<stanza-name> check
    ```

1. 백업 개시
    ```sh
    # FULL
    pgbackrest --stanza=<stanza-name> backup --type=full
   
    # INCREMENT
    pgbackrest --stanza=<stanza-name> backup --type=incr

    # DIFF
    pgbackrest --stanza=<stanza-name> backup --type=diff
    ```

1. 백업 정보 확인
    ```
    pgbackrest --stanza=<stanza-name> info
    ```

<br>

### 복원
1. 백업 정보 확인
    ```
    pgbackrest --stanza=<stanza-name> info
    ```

1. 복원 개시
    ```
    pgbackrest --stanza=<stanza-name> restore --type=<type-name>
    ```
   
    |타입|설명|
    |-|-|
    |`default`|Archive Stream까지 복원|
    |`immediate`|데이터베이스의 일관성이 있을 때 까지 복원|
    |`lsn`|해당 LSN 번호까지 복원|
    |`name`|`--target` 으로 정의한 백업 이름을 복원|
    |`xid`|`--target` 으로 정의한 TRANSACTION ID까지 복원|
    |`time`|`--target` 으로 정의한 Timestamp 시점까지 복원 (`yyyy-mm-dd hh24:mi:ss+UTC TIME`)|
    |`preserve`|기존의 `postgresql.auto.conf`, `recovery.conf` 파일을 보존|
    |`standby`|`postgresql.auto.conf`, 또는`recovery.conf`애 `standby_mode=on` 을 추가하여 클러스터가 STANDBY 모드로 시작하도록 함|
    |`none`|`postgresql.auto.conf`, 또는 `recovery.conf` 을 작성하지 않고 `pg_xlog/pg_wal` 에 있는 WAL SEGMENT를 사용해 일관성을 유지 (필요한 WAL SEGMENT의 제공, 또는 `archive-copy` 설정을 사용할 필요가 있음)|
    >`none` 인 경우 복원이 끝날 때까지 Timeline가 증가하지 않기 때문에 주의해야함 (중복된 WAL을 Archive하게 되면 디스크 풀, 또는 `pg_rewind` 의 동작 에러가 발생할 가능성이 있음)

<br>

### 백업 삭제
1. 백업 정보 확인
    ```
    pgbackrest --stanza=<stanza-name> info
    ```

1. 해당 백업 삭제
    ```
    pgbackrest --stanza=<stanza-name> expire --set=<backup-name>
    ```

<br>

### STANZA 삭제
1. 클러스터 정지
    ```
    pg_ctlcluster <cluster-name> stop
    ```

1. STANZA 정지
    ```
    pgbackrest --stanza=<stanza-name> stop
    ```

1. STANZA 삭제
    ```
    pgbackrest --stanza=<stanza-name> stanza-delete
    ```

<br>
