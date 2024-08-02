Archive
===

### 아카이브 로그
오라클은 데이터 복구를 위해 데이터베이스에 대한 모든 변경사항을 redo 로그 파일에 보관함

|아카이브 모드|설명|
|-|-|
|Enabled|리두 로그 파일을 보관함|
|Disabled|리두 로그 파일을 보관하지 않음|

아카이브 모드가 활성화가 되어있지 않다면 몇개의 redo 로그 파일로 돌려 쓰는 방식 (이전 정보는 사라짐)

활성화가 되어있다면 이전 파일을 다른 곳에 복사해 두는 방식 (모든 정보가 보존)

<br>

### 아카이브 모드 설정
1. 아카이브 로그를 저장할 디렉토리를 생성
    ```sh
    mkdir /ora_archive_1
    mkdir /ora_archive_2
    mkdir /ora_archive_3

    chown -R oracle_user:group_name /ora_archive_1
    chown -R oracle_user:group_name /ora_archive_2
    chown -R oracle_user:group_name /ora_archive_3
    ```
    >아카이브 로그를 분산시켜 만일의 사태에 대비함

1. 데이터베이스에 접속 후 아카이브 로그 파일이 저장될 위치를 지정하는 파라미터를 수정
    ```sql
    sqlplus / as sysdba

    --Set Parameter
    ALTER SYSTEM SET log_archive_dest_1 = 'location=/ora_archive_1/' SCOPE = spfile;
    ALTER SYSTEM SET log_archive_dest_2 = 'location=/ora_archive_2/' SCOPE = spfile;
    ALTER SYSTEM SET log_archive_dest_3 = 'location=/ora_archive_3/' SCOPE = spfile;
    ```
    >`log_archive_dest_n`은 아카이브 로그 파일을 최대 10개까지 서로 다른 폴더에 분산시킬 수 있음

1. 가동 중인 데이터베이스 종료 후 마운트 단계까지 기동
    ```sql
    --Shutdown
    SHUTDOWN IMMEDIATE;
    
    --Start Mount
    STARTUP MOUNT;
    ```

1. 아카이브 모드를 변경 후 오픈
    ```sql
    --Set Archive Mode Enabled
    ALTER DATABASE archivelog;

    --Start Open
    ALTER DATABASE OPEN;
    ```

1. 아카이브 모드 확인
    ```sql
    archive log list;
    /*
    Database log mode            Archive Mode
    Automatic archival           Enabled
    Archive destination          USE_DB_RECOVERY_FILE_DEST
    Oldest online log sequence   34
    Next log sequence to archive 35
    Current log sequence         35
    */

    --강제로 로그 스위치
    ALTER SYSTEM SWITCH logfile;
    ```

<br>

### 아카이브 모드 해제
1. 가동 중인 데이터베이스 종료 후 마운트 단계까지 기동
    ```sql
    --Shutdown
    SHUTDOWN IMMEDIATE;
    
    --Start Mount
    STARTUP MOUNT;
    ```

1. 아카이브 모드를 변경 후 오픈
    ```sql
    --Set Archive Mode Disabled
    ALTER DATABASE noarchivelog;

    --Start Open
    ALTER DATABASE OPEN;
    ```

1. 아카이브 모드 확인
    ```sql
    archive log list;
    ```

<br>

### 아카이브 로그 삭제
1. RMAN 접속
    ```sql
    --오라클 유저로 변경
    su - oracle_user;

    --RMAN 접속
    rman target /
    ```

1. 아래 명령어를 확인해 필요한 작업 실행
    ```sql
    --아카이브 로그 리스트 확인
    list archivelog all;
    
    --아카이브 로그 삭제 (Expired는 삭제되지 않음)
    delete archivelog all;

    --시간 기준으로 아카이브 로그 삭제
    delete archivelog until time 'sysdate - 7' all;

    --Expired된 아카이브 로그 삭제
    delete expired archivelog all;

    --아카이브 로그 크로스체크
    crosscheck archivelog all;
    ```

<br>
