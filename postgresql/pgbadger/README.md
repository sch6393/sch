PGBADGER
===
>https://github.com/darold/pgbadger

### 설치
```sh
# 압축 해제
tar zxvf /pgbadger-12.4.tar.gz -C /tmp
cd /tmp/pgbadger-12.4

# perl
perl Makefile.PL

# make
make && make install

# Check Version
pgbadger -V

# Delete Temparary Files
rm -rf /tmp/pgbadger-12.4
```

<br>

### 필요한 파라미터
```yml
log_autovacuum_min_duration: 0
log_checkpoints: on
log_connections: on
log_disconnections: on
log_line_prefix: '%t [%p - %l]: error=%e, %qdb=%d, user=%u, remote=%r, app=%a, client=%h, '
log_lock_waits: on
log_min_duration_statement: 100 # 단위 : 1/1000초, 0 : 모든 쿼리를 로깅
log_temp_files: 0
```

<br>

### 사용
```sh
# 기본
pgbadger --prefix 'log_line_prefix' /directroy/postgresql-log-file

# 로그 파일 여러개를 읽기
pgbadger --prefix '%t [%p - %l]: error=%e, %qdb=%d, user=%u, remote=%r, app=%a, client=%h, ' /var/pgsql/data/log/postgresql-2024-12-03_14.log /var/log/postgres/postgresql-2024-12-03_15.log -o /root/0.html

# 시간지정
pgbadger --prefix '%t [%p - %l]: error=%e, %qdb=%d, user=%u, remote=%r, app=%a, client=%h, ' -b "2024-12-03 14:50:00" -e "2024-12-03 15:01:00" /var/log/postgres/postgresql-2024-12-03_*.log -o /root/1.html

# 레포트 작성 시 사용할 CPU 코어 갯수 지정
pgbadger --prefix '%t [%p - %l]: error=%e, %qdb=%d, user=%u, remote=%r, app=%a, client=%h, ' -b "2024-12-03 14:50:00" -e "2024-12-03 15:01:00" /var/log/postgres/postgresql-2024-12-03_*.log -j 2 -o /root/2.html

# 레포트 작성을 매일, 매주 단위로 작성하도록 함 (Incremental 모드 지정)
pgbadger --prefix '%t [%p - %l]: error=%e, %qdb=%d, user=%u, remote=%r, app=%a, client=%h, ' /var/log/postgres/postgresql-*.log -j 2 -I -E -O /home/postgres/report

# 월간 레포트 작성
pgbadger –month-report 2024-12 /var/log/postgres/postgresql-*.log -j 2 -O /home/postgres/report

# 월간 레포트 작성 (-E)
pgbadger -E –month-report 2024-12 /home/postgres/report
```

<br>

### 옵션
|OPTION|VALUE|DESCRIPTION|
|-|-|-|
|`-a`|`number`|Period 지정 (단위 : 분)|
|`-b`|`datetime`, `timestamp`|특정시간지정 (개시)|
|`-e`|`datetime`, `timestamp`|특정시간지정 (종료)|
|`-E`||각 데이터베이스 별 표시|
|`-I`||Incremental 모드 활성화|
|`-j`|`number`|CPU Core 지정|
|`–month-report`|`yyyy-mm`|월간 레포트 작성|
|`-o`|`/directory/filename`|파일 Output 지정|
|`-O`|`/directory`|파일 Output 디렉토리 지정|
|`--prefix`|`log_line_prefix`|Log 파일의 Prefix 지정|
|`--retention`|`number`|남겨둘 레포트 기간 (단위 : 주)|
|`-v`||디버그 모드|

<br>

### 체크 항목
|항목|
|-|
|__Overview__|
|__Connections__|
|__Sessions__|
|Checkpoints|
|Temp Files|
|Vacuums|
|Locks|
|Queries|
|__Top__|
|__Events__|

<br>
