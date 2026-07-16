mysqlbinlog
===
>[https://dev.mysql.com/doc/refman/8.0/en/mysqlbinlog.html](https://dev.mysql.com/doc/refman/8.0/en/mysqlbinlog.html)

### 기본 형식
```sql
mysqlbinlog binary_log.000000 > file_name.sql
```

<br>

### 바이너리 로그 반영
```sql
mysql -u user_name -p < file_name.sql
```

<br>

### 옵션
|옵션|설명|예시|
|-|-|-|
|`--no-defaults`|my.cnf 파일을 읽지 않고 실행||
|`--database`|스키마 이름 지정|`--database=schema_name`|
|`--start-datetime`|특정 시간 지정 (시작 시간)|`--start-datetime="2023-03-10 06:00:00"`|
|`--stop-datetime`|특정 시간 지정 (종료 시간)|`--stop-datetime="2023-03-10 09:00:00"`|
|`--start-position`|특정 포지션 지정 (시작 포지션)|`--start-position=0000000`|
|`--stop-position`|특정 포지션 지정 (종료 포지션)|`--stop-position=0000000`|
|`-v`|매칭되는 컬럼의 개수와 값 표시||
|`-vv`|매칭되는 컬럼의 개수와 값, 데이터 타입 표시||
|`--base64-output=DECODE-ROWS`|`BINLOG` 문 제외|`-v` 옵션을 같이 쓰면 SQL 구문 확인 가능|
|`--read-from-remote-server`|원격 서버로부터 Binary log 파일을 읽음|`--read-from-remote-server --host=host_name --user=user_name --password=password`|

<br>
