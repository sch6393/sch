Install Oracle Client
===

### 쉘 설치
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

### GUI 설치
1. https://www.oracle.com/database/technologies/oracle21c-linux-downloads.html

1. `unzip LINUX.X64_213000_client.zip ./oracleClient21/`

1. `client` 폴더 안의 `runInstaller` 실행

<br>

* 에러 메시지 별 해결책
  ```
  checking swap space 0 mb available 150 mb required. failed
  > 스왑 공간 설정

  Could not execute auto check for display colors using command /usr/bin/xdpyinfo. Check if the DISPLAY variable is set.
  > export DISPLAY=:1 (환경 변수 설정)

  Bash Profile에 설정했음에도 똑같은 에러가 발생한다면 xdpyinfo가 설치되어있는지 확인
  > /usr/bin/xdpyinfo (확인)
  > yum  install xdpyinfo (설치)

  Invalid MIT-MAGIC-COOKIE-1 keyxhost: unable to open display ":1"
  > 해당 세션과 XAuth 쿠키 값이 맞는지 확인 -> VNC를 종료하고 다시 실행
  ```

<br>

### Profile 설정
1. Profile 디렉토리에 설정
    ```sh
    # 쉘 설치일 경우
    sudo vi /etc/profile.d/oracle.sh

    export ORACLE_HOME=/usr/lib/oracle/21/client64
    export TNS_ADMIN=$ORACLE_HOME/bin
    export PATH=$ORACLE_HOME/bin:$PATH
    ```

1. 유저 Bash Profile에 설정
    ```sh
    sudo vi ~/.bash_profile

    export ORACLE_HOME=/usr/lib/oracle/21/client64
    export TNS_ADMIN=$ORACLE_HOME/bin
    export PATH=$ORACLE_HOME/bin:$PATH
    ```

<br>
