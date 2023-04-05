mysqldump
===

### 기본 형식
```sh
mysqldump -u USER -p > /dumpfile.sql

# 데이터 없이 데이터베이스 구조만 가져오는 경우
mysqldump -u USER -p --databases SCHEMA_NAME1 SCHEMA_NAME2 --no-data --routines --triggers > backup.sql
```

<br>

### 덤프 속도와 복원 속도를 최대한 빠르게 하기 위한 절차
1. 덤프 시 넣는 옵션
    ```sh
    mysqldump --default-character-set=utf8mb4 --all-databases --routines --triggers --quick --extended-insert --single-transaction --no-autocommit --hex-blob > /dumpfile.sql
    ```

2. 복원 인스턴스에 [관련 파라미터](#관련-파라미터) 를 서버 리소스에 맞게 수정
    ```sql
    SET GLOBAL max_allowed_packet = ;
    SET GLOBAL net_buffer_length = ;
    SET GLOBAL bulk_insert_buffer_size = ;
    SET GLOBAL key_buffer_size = ;
    SET GLOBAL innodb_buffer_pool_size = ;
    SET GLOBAL innodb_flush_log_at_trx_commit = 0;
    SET GLOBAL foreign_key_checks = 0;
    SET GLOBAL unique_checks = 0;

    --복원이 끝나면 1로 변경
    SET GLOBAL innodb_flush_log_at_trx_commit = 1;
    SET GLOBAL foreign_key_checks = 1;
    SET GLOBAL unique_checks = 1;
    ```

3. 복원 시작
    ```sh
    mysql -u USER -p < /dumpfile.sql
    ```

<br>

### 여러 옵션 설명
```sh
# 특정 호스트 및 포트 지정
mysqldump -h HOST -P PORT -u USER -p > /dumpfile.sql

# 캐릭터 셋 지정 (기본 값 : my.cnf에 정의한 캐릭터 셋)
mysqldump -u USER -p --default-character-set=cp932 > /dumpfile.sql

# binary
mysqldump -u USER -p --default-character-set=binary > /dumpfile.sql

# 전체 데이터베이스 (-A)
mysqldump -u USER -p --all-databases > /dumpfile.sql

# 특정 스키마 (-B)
mysqldump -u USER -p --databases SCHEMA_NAME1 SCHEMA_NAME2 > /dumpfile.sql

# 테이블 락 O (-l)
mysqldump -u USER -p --lock-tables > /dumpfile.sql

# 전체 테이블 락 O (-x)
mysqldump -u USER -p --lock-all-tables > /dumpfile.sql

# 테이블 락 X (트랜잭션 테이블만 적용, MyISAM과 HEAP은 해당되지 않음)
mysqldump -u USER -p --single-transaction > /dumpfile.sql

# 임포트가 끝난 후 인덱스 활성화 (-K) (MyISAM만 가능)
mysqldump -u USER -p --disable-keys > /dumpfile.sql

# 다중 행 Insert 사용 (-e)
mysqldump -u USER -p --extended-insert > /dumpfile.sql

# 마스터 포지션 기록 (--single-transaction 옵션과 같이 쓰지 않으면 전체 테이블 락이 발생)
mysqldump -u USER -p --master-data > /dumpfile.sql

# 커밋을 테이블 단위로 하게 함
mysqldump -u USER -p --no-autocommit > /dumpfile.sql

# 통신용 버퍼 크기 지정 (최대 값 : 1GB) (my.cnf와 동일해야 함)
mysqldump -u USER -p --max_allowed_packet=1G > /dumpfile.sql

# 통신용 버퍼의 초기화 크기 지정 (max_allowed_packet 값보다 작아야 함)
mysqldump -u USER -p --net_buffer_length=512M > /dumpfile.sql

# 트리거 미포함
mysqldump -u USER -p --skip-triggers > /dumpfile.sql

# 트리거 포함
mysqldump -u USER -p --triggers > /dumpfile.sql

# 함수, 프로시저 미포함
mysqldump -u USER -p --skip-routines > /dumpfile.sql

# 함수, 프로시저 포함 (-R)
mysqldump -u USER -p --routines > /dumpfile.sql

# TIMESTAMP 데이터를 그대로 적용
mysqldump -u USER -p --tz-utc > /dumpfile.sql

# 조건에 맞는 데이터 (-w'')
mysqldump -u USER -p --where='column_name < 100' > /dumpfile.sql

# 데이터를 메모리에 로딩하지 않고 직접 읽게 함 (-q)
mysqldump -u USER -p --quick > /dumpfile.sql

# 임포트 성능 향상을 위해 LOCK 구문 삽입
mysqldump -u USER -p --add-locks > /dumpfile.sql

# CREATE TABLE 전에 DROP TABLE 구문 삽입
mysqldump -u USER -p --add-drop-table > /dumpfile.sql

# 해당 옵션은 --quick, --add-drop-table, --add-locks, --extended-insert, --lock-tables과 같음
mysqldump -u USER -p --opt > /dumpfile.sql


# Slave에서 덤프를 할 경우 (데이터는 Slave이지만 스냅샷 정보는 Master에서 가져오게 됨)
mysqldump -u USER -p --dump-slave > /dumpfile.sql

# --dump-slave 옵션인 경우 마스터 정보를 같이 포함
mysqldump -u USER -p --dump-slave --include-master-host-port > /dumpfile.sql


# BLOB 타입
mysqldump -u USER -p --hex-blob > /dumpfile.sql
```

<br>

### 관련 파라미터
```sh
# 통신용 버퍼 크기 지정 (최대 값 : 1GB) (my.cnf와 동일해야 함)
max_allowed_packet=1G

# 통신용 버퍼의 초기화 크기 지정 (max_allowed_packet 값보다 작아야 함)
net_buffer_length=256M

# 다중 행 Insert (--extended-insert 옵션) 사용했을 때 최대로 작성 가능한 레코드 길이 지정 (max_allowed_packet 값보다 작아야 함)
bulk_insert_buffer_size=256M

# 서버 메모리의 약 60%를 할당
key_buffer_size=512M

# 서버 메모리의 약 50 ~ 80%를 할당
innodb_buffer_pool_size=1G

# Insert 시 로그를 기록 (1 : ON, 0 : OFF)
innodb_flush_log_at_trx_commit=0

# 외래키 체크 (1 : ON, 0 : OFF)
foreign_key_checks=0

# 유니크 체크 (1 : ON, 0 : OFF)
unique_checks=0
```

<br>

### [복원](../mysql/README.md#덤프-파일로부터-복원)

<br>
