Install Oracle Client
===

### 설치 명령어
```sh
cd /usr/local/src

sudo wget https://download.oracle.com/otn_software/linux/instantclient/215000/oracle-instantclient-basic-21.5.0.0.0-1.x86_64.rpm
sudo wget https://download.oracle.com/otn_software/linux/instantclient/215000/oracle-instantclient-sqlplus-21.5.0.0.0-1.x86_64.rpm
sudo wget https://download.oracle.com/otn_software/linux/instantclient/215000/oracle-instantclient-tools-21.5.0.0.0-1.x86_64.rpm

sudo yum install oracle-instantclient-basic-21.5.0.0.0-1.x86_64.rpm -y
sudo yum install oracle-instantclient-sqlplus-21.5.0.0.0-1.x86_64.rpm -y
sudo yum install oracle-instantclient-tools-21.5.0.0.0-1.x86_64.rpm -y
```

<br>

### profile 설정
1. Profile 디렉토리에 설정
    ```sh
    sudo vi /etc/profile.d/oracle.sh

    export ORACLE_HOME=/usr/lib/oracle/21/client64
    export TNS_ADMIN=$ORACLE_HOME/bin
    export PATH=$ORACLE_HOME/bin:$PATH
    ```

2. 유저 Bash Profile에 설정
    ```sh
    sudo vi ~/.bash_profile

    export ORACLE_HOME=/usr/lib/oracle/21/client64
    export TNS_ADMIN=$ORACLE_HOME/bin
    export PATH=$ORACLE_HOME/bin:$PATH
    ```