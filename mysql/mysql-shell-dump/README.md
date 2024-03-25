mysql shell dump
===

### 사전 준비
[MySQL Shell](../mysqlshell/README.md) 설치가 필요 

<br>

### 동작 순서
1. 바이너리 로그 포지션 기억
    ```sql
    FLUSHNO_WRITE_TO_BINLOG TABLES
    FLUSH TABLES WITHREAD LOCK
    SET SESSIONTRANSACTION ISOLATION LEVEL REPEATABLE READ
    START TRANSACTIONWITH CONSISTENT SNAPSHOT

    SELECT @@GLOBALGTID_EXECUTED

    SHOW MASTER STATUS

    LOCK INSTANCE FORBACKUP
    UNLOCK TABLES
    ```
1. 오브젝트 DDL 생성
1. 스레드 수 만큼 Dump 작업

<br>

### 덤프 파일 구조
|파일|설명|
|-|-|
|`@.json`|백업 시작 정보 (백업 대상, 사용한 옵션, 바이너리 로그 포지션 등)|
|`schema_name.json`|헤당 스키마에 대한 오브젝트 정보|
|`schema_name@table_name.json`|해당 테이블의 정보|
|`*.sql`|해당 오브젝트의 DDL 정보|
|`schema_name@table_name.tsv.*`|해당 테이블의 데이터 정보|
|`schema_name@table_name.tsv.*.idx`|해당 테이블의 인덱스 정보|
|`@.done.json`|백업 완료 정보 (백업 대상 크기, 테이블 chunk 개수, 크기 등)|

<br>

### 백업 기본 형식
```sh
# Shell 접속할 경우
# 인스턴스 단위
util.dumpInstance("/directory_name", {option_name1:value, option_name2:value, ... })

# 스키마 단위
util.dumpSchemas("schema_name", "/directory_name", {option_name1:value, option_name2:value, ... })

# 테이블 단위
util.dumpTables("schema_name", ["table_name1", "table_name2", ... ], "/directory_name", {option_name1:value, option_name2:value, ... })


# Shell 접속하지 않을 경우 -e 옵션으로 명령어 지정
mysqlsh root@localhost -e \
'util.dumpInstance("/directory_name", {option_name1:value, option_name2:value, ... })'
```

<br>

### 복원 기본 형식
```sh
# Shell 접속 후
util.loadDump("/directory_name", {option_name1:value, option_name2:value, ... })
```

<br>

### 속도 개선
1. 복원 시에 `skipBinlog`, `thread` 옵션 지정

1. redo_log를 비활성화해서 Load 속도를 개선할 수 있음 (단, `8.0.21` 이상이여야 함)
    ```sql
    # 비활성화
    ALTER INSTANCE DISABLE INNODB REDO_LOG;

    # 확인
    SHOW GLOBAL STATUS LIKE 'innodb_redo_log_enabled';
    /*
    +-------------------------+-------+
    | Variable_name           | Value |
    +-------------------------+-------+
    | Innodb_redo_log_enabled | OFF   |
    +-------------------------+-------+
    */

    # 활성화
    ALTER INSTANCE ENABLE INNODB REDO_LOG;

    # 확인
    SHOW GLOBAL STATUS LIKE 'innodb_redo_log_enabled';
    /*
    +-------------------------+-------+
    | Variable_name           | Value |
    +-------------------------+-------+
    | Innodb_redo_log_enabled | ON    |
    +-------------------------+-------+
    */
    ```

<br>

### 주의점
1. PK가 없다면 chunk 단위로 병렬 백업, 복원이 불가능함 (하나의 파일로 작업)
1. 압축 시 CPU 사용율이 높으므로 Thread 지정이나 작업할 시간대를 확인해야 함
1. 복원을 하기 위해선 [`local_infile`](../parameter/local_infile.md) 파라미터가 `ON` 으로 되어있어야 함
1. mysqldump와는 상호 호환되지 않음

<br>

### 백업 옵션 설명
|OPTION|DESCRIPTION|
|-|-|
|`thread`|지정한 스레드 수 만큼 병렬로 작업|
|`compression`|`zstd` : zstd 방식으로 압축 (기본 값)<br>`gzip` : gzip 방식으로 압축|
|`dryRun`|`true` : 실행 전 호환성 체크를 함 (Dump가 실행되지는 않음)<br>`false` : 실행 전 호환성 체크를 하지 않음 (Dump가 실행되지는 않음) (기본 값)|
|`showProgress`|`true` : Dump 실행 시 진행 정보 출력 (기본 값)<br>`false` : 진행 정보 출력 하지 않음|
|`maxRate `|읽기 처리량에 대해 스레드 당 바이트 크기를 지정 (초단위)|
|`defaultCharacterSet`|세션에서 사용할 문자 집합 (기본 값 : `utf8mb4`)|
|`consistent`|`true` : 일관성 Dump를 수행함 (기본 값)<br>`false` : 일관성 Dump를 수행 하지 않음|

<br>

|AWS S3 관련 옵션|설명|
|-|-|
|`s3BucketName`|S3 버킷 지정|
|`s3CredentialsFile`|S3 Credentials 파일 지정 (파일 디렉토리와 이름이 기본값이라면 불필요)|
|`s3ConfigFile`|S3 Config 파일 지정 (파일 디렉토리와 이름이 기본값이라면 불필요)|
|`s3Profile`|S3 Profile 지정|
|`s3Region`|S3 Region 지정 (Config 파일에 설정되어 있다면 불필요)|
|`s3EndpointOverride`||
>[AWS CLI](../../aws/cli/README.md)를 설치, 설정해야 하고 해당 계정에 대해 [S3](../../aws/s3/README.md) 권한이 있어야 함
```sh
mysqlsh root@localhost -e 'util.dumpInstance("Folder Name",{s3bucketName: "Bucket Name"})'
```

<br>

### 복원 옵션 설명
|OPTION|DESCRIPTION|
|-|-|
|`thread`|지정한 스레드 수 만큼 병렬로 작업|
|`skipBinlog`|`true` : Load할 때 바이너리 로그를 비활성화<br>`false` : Load할 때 바이너리 로그를 활성화|
|`ignoreExistingObjects`|`true` : 테이블이 존재할 경우 무시하고 데이터를 Load<br>`false` : 테이블이 존재할 경우 에러 표시 (기본 값)|
|`ignoreVersion `|`true` : Dump한 데이터베이스 버전과 Load할 데이터베이스 버전이 달라도 진행<br>`false` : 데이터베이스 버전이 다르다면 에러 표시|
|`resetProgress`|`true` : 중단 후 다시 시작할 때 이어서 진행함<br>`false` : 중단 후 다시 시작할 때 처음부터 진행함|

<br>

### 참고
* [GTID](../gtid/README.md)
* [mysqldump](../mysqldump/README.md)
* [mysqldump, mysqlpump, mydumper, mysql shell dump의 차이](../mysqldump-pump-dumper-shell/README.md)

<br>
