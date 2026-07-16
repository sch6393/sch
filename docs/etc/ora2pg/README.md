ora2pg
===
>https://ora2pg.darold.net/index.html

### 동작 환경 구성 (Linux)
1. [Install Oracle Client](../../oracle/install-oracle-client/README.md)

1. `perl` 버전 확인
    ```sh
    perl -v

    # This is perl 5, version 32, subversion 1 (v5.32.1) built for x86_64-linux-thread-multi
    # (with 51 registered patches, see perl -V for more detail)
    #
    # Copyright 1987-2021, Larry Wall
    #
    # Perl may be copied only under the terms of either the Artistic License or # the
    # GNU General Public License, which may be found in the Perl 5 source kit.
    #
    # Complete documentation for Perl, including FAQ lists, should be found on
    # this system using "man perl" or "perldoc perl".  If you have access to the
    # Internet, point your browser at http://www.perl.org/, the Perl Home Page.
    ```
    >`5.10` 이상 필요

1. `perl-CPAN` 설치
    ```sh
    dnf install perl-CPAN
    ```

1. 환경변수 설정
    ```sh
    export PERL5LIB=/usr/local/bin
    export LD_LIBRARY_PATH=/usr/lib/oracle/21/client64/lib
    export ORACLE_HOME=/usr/lib/oracle/21/client64
    ```
    >`ORACLE_HOME` 의 경우 오라클 클라이언트 설치할 때 프로파일 정의했으면 상관 없음

1. 필요한 Module 설치
    ```sh
    perl -MCPAN -e 'install Test::NoWarnings'
    perl -MCPAN -e 'install DBI'
    perl -MCPAN -e 'install DBD::Oracle'
    ```

1. ora2pg 패키지 다운로드 후 압축 풀기
    ```sh
    cd /usr/local/src
    tar xzf ora2pg-24.0.tar.gz
    ```

1. 압축 푼 디렉토리로 이동 후 ora2pg 설치
    ```sh
    cd ora2pg-24.0
    perl Makefile.PL
    make && make install
    ```

1. 설정 파일 복사
    ```sh
    cp /etc/ora2pg/ora2pg.conf.dist /etc/ora2pg/ora2pg.conf
    ```

1. 오라클 연결 정보 입력
    ```sh
    vi /etc/ora2pg/ora2pg.conf

    # Set the Oracle home directory
    ORACLE_HOME     /usr/lib/oracle/21/client64

    # Set Oracle database connection (datasource, user, password)
    ORACLE_DSN      dbi:Oracle:HOST_NAME;sid=SID_NAME;port=1521
    ORACLE_USER     admin
    ORACLE_PWD      password
    ```
    >접속하는 유저는 DBA 권한이 있어야 함

<br>

### 설정 옵션
* 소스 데이터베이스가 `AL32UTF8` 또는 `UTF8`이 아닌 경우
  * `NLS_LANG` 과 `BINMODE` 를 설정해야 함

<br>
