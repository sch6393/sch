Active Duplication
===

### 사전준비
* 11.2.0.4 패치
  * [Patch](../patch/README.md)

<br>

### 1. TNS 설정 (Primary, Standby)
* Primary, Standby 모두 추가
  ```sql
  # ACTIVE DUPLICATION
  PRIMARY =
    (DESCRIPTION =
      (ADDRESS_LIST =
        (ADDRESS = (PROTOCOL = TCP)(HOST = 0.0.0.1)(PORT = 1521))
      )
      (CONNECT_DATA =
        (SERVER_NAME = DEDICATED)
        (SERVICE_NAME = TEST)(UR=A)
      )
    )

  STANDBY =
    (DESCRIPTION =
      (ADDRESS_LIST =
        (ADDRESS = (PROTOCOL = TCP)(HOST = 0.0.0.2)(PORT = 1521))
      )
      (CONNECT_DATA =
        (SERVER_NAME = DEDICATED)
        (SERVICE_NAME = TEST)(UR=A)
      )
    )
  ```
  >(UR=A) : Standby에서 nomount로 기동했을 시 리스너에서 막히는 것을 방지하는 옵션

<br>

### 2. TNS PING 테스트 (Primary, Standby)
* Primary, Standby 모두 테스트
  ```
  tnsping PRIMARY
  tnsping STANDBY
  ```
  >4개 전부 통신 성공해야함

<br>

### 3. 데이터베이스 설정 확인 (Primary)
* 아카이브 모드 활성화 확인
  ```sql
  archive log list;
  --Database log mode              Archive Mode
  ```

* 강제 로그 활성화
  ```sql
  SELECT FORCE_LOGGING, SUPPLEMENTAL_LOG_DATA_MIN FROM V$DATABASE; --19c
  SELECT FORCE_LOGGING FROM V$DATABASE;
  ALTER DATABASE FORCE LOGGING;
  ```

* EXCLUSIVE로 되어있는지 확인
  ```sql
  SHOW PARAMETER REMOTE_LOGIN_PASSWORDFILE;
  --remote_login_passwordfile            string      EXCLUSIVE
  ```

* pfile 수정
  ```sql
  create pfile from spfile;
  ```
  ```
  *.db_name='TEST'
  *.db_unique_name='primary'
  *.log_archive_config='dg_config=(primary,standby)'
  *.log_archive_dest_1='LOCATION=/data/oracle11g/oraarchive/TEST'
  *.log_archive_dest_2='service=standby lgwr sync valid_for=(ONLINE_LOGFILES,PRIMARY_ROLE) db_unique_name=standby'
  *.log_archive_dest_state_1='enable'
  *.log_archive_dest_state_2='enable'
  *.standby_file_management='AUTO'
  ```

* pfile에서 변경한 내용을 spfile에도 적용
  ```sql
  --셧다운
  shutdown

  --pfile로 재기동
  startup pfile ='/oracle/oracle11g/product/11.2.0.4/dbhome/dbs/initTEST.ora'
  create spfile from pfile;

  --spfile로 재기동
  shutdown
  startup

  --DB 이름 확인
  SHOW PARAMETER DB_NAME;
  SHOW PARAMETER DB_UNIQUE_NAME; --primary로 바뀌어야 함
  ```

<br>

### 4. 패스워드 파일 이동 (선택사항)
* Primary ➞ Standby
  * 파일경로 : `/oracle/oracle11g/product/11.2.0.4/dbhome/dbs/orapwTEST`
    >인스턴스를 같은 비밀번호로 생성했다면 스킵해도 문제가 없음

<br>

### 5. 데이터베이스 설정 확인 (Standby)
* pfile 수정
  ```sql
  create pfile from spfile;
  ```
  ```
  *.db_name='TEST'
  *.db_unique_name='standby'
  *.log_archive_config='dg_config=(primary,standby)'
  *.log_archive_dest_1='LOCATION=/data/oracle11g/oraarchive/TEST'
  *.log_archive_dest_2='service=standby lgwr async valid_for=(ONLINE_LOGFILES,PRIMARY_ROLE) db_unique_name=standby'
  *.log_archive_dest_state_1='enable'
  *.log_archive_dest_state_2='enable'
  *.standby_file_management='AUTO'
  *.fal_client='primary'
  *.fal_server='standby'
  ```

* pfile에서 변경한 내용을 spfile에도 적용
  ```sql
  --셧다운
  shutdown

  --pfile로 재기동
  startup pfile ='/oracle/oracle11g/product/11.2.0.4/dbhome/dbs/initTEST.ora'
  create spfile from pfile;

  --spfile로 재기동
  shutdown
  startup

  --DB 이름 확인
  SHOW PARAMETER DB_NAME;
  SHOW PARAMETER DB_UNIQUE_NAME; --standby로 바뀌어야 함
  ```

* no mount로 기동
  ```
  shutdown
  startup nomount
  ```
>Standby의 pfile의 나머지 파라미터들을 Primary에 맞춰서 바꿔주는 것을 권장

<br>

### 6. Duplication 시작 (Primary)
* 상태 체크
  ```sql
  SELECT DATABASE_ROLE, PROTECTION_MODE FROM v$database;
  --DATABASE_ROLE    PROTECTION_MODE
  ------------------ --------------------
  --PRIMARY          MAXIMUM PERFORMANCE
  ```

* RMAN 접속
  ```sql
  rman target sys/oracle@PRIMARY auxiliary sys/oracle@STANDBY

  --connected to target database: TEST (DBID=1467293175)
  --connected to auxiliary database: TEST (not mounted)

  --듀플리케이션 실행
  run {
  allocate channel primary type disk
  allocate auxiliary channel standby type disk
  duplicate target database for standby from active database
  ;
  }
  ```

<br>

### 7. Duplication 설정 (Primary, Standby)
* redolog 설정 (Primary, Standby 모두 추가)
  ```sql
  ALTER DATABASE ADD STANDBY LOGFILE GROUP 4 SIZE 500M;
  ALTER DATABASE ADD STANDBY LOGFILE GROUP 5 SIZE 500M;
  ALTER DATABASE ADD STANDBY LOGFILE GROUP 6 SIZE 500M;
  ```

* Primary 
  ```sql
  ALTER DATABASE ADD SUPPLEMENTAL LOG DATA; --현재 시점부터 DML을 기록
  ```
  >LGWR

* Standby
  ```sql
  alter database recover managed standby database disconnect from session;
  alter database recover managed standby database cancel;

  alter database open; --듀플리케이션하면서 인스턴스가 자동으로 시작됨

  alter database recover managed standby database using current logfile disconnect;
  ```

<br>

### 8. Duplication 확인 (Primary, Standby)
* Primary
  ```sql
  select database_role, open_mode FROM v$database;
  --DATABASE_ROLE    OPEN_MODE
  ------------------ --------------------
  --PRIMARY          READ WRITE
  ```

* Standby
  ```sql
  select database_role, open_mode FROM v$database;
  --DATABASE_ROLE    OPEN_MODE
  ------------------ --------------------
  --PHYSICAL STANDBY READ ONLY WITH APPLY

  --데이터가드 연결이 되어 있는지 확인
  select PROCESS,STATUS,CLIENT_PROCESS,THREAD#,SEQUENCE#,BLOCK# from v$managed_standby where process = 'MRP0' or client_process='LGWR';
  PROCESS   CLIENT_P    THREAD#  SEQUENCE#     BLOCK#
  --------- -------- ---------- ---------- ----------
  MRP0      N/A               1         34      12118
  RFS       LGWR              1         34      12118
  ```

* Primary
  ```sql
  --아카이브 로그 시퀸스 강제 추가 (확인용)
  alter system archive log current;

  --시퀸스 확인
  select max(sequence#) from v$thread;
  ```

* Standby
  ```sql
  --시퀸스 확인 (Primary와 같아야함)
  select max(sequence#) from v$thread;
  ```
