pgbench
===
>https://www.postgresql.org/docs/current/pgbench.html

### 설치
기본적으로 설치되어 있으므로 별도 설치할 필요가 없음

<br>

### 설정
1. pgbench 전용 데이터베이스 작성
    ```sql
    CREATE DATABASE pgbench;
    ```

1. 테스트 데이터 작성
    ```sh
    pgbench -i pgbench -s <number> # FACTOR：10만 레코드
    ```

<br>

### 테스트
```sh
pgbench <pgbench_schema_name> <option> <value> ...

# 예시
pgbench pgbench -c 70 -j 2 -T 30
#starting vacuum...end.
#transaction type: <builtin: TPC-B (sort of)>
#scaling factor: 100
#query mode: simple
#number of clients: 70
#number of threads: 2
#duration: 30 s
#number of transactions actually processed: 136493
#latency average = 15.416 ms
#tps = 4540.590199 (including connections establishing)
#tps = 4540.895216 (excluding connections establishing)
```

|OPTION|VALUE|DESCRIPTION|
|-|-|-|
|`-c`|`number`|가상 클라이언트 수|
|`-d`||DEBUG 모드|
|`-f`|`.sql` 파일|테스트할 SQL 파일 지정|
|`-j`|`number`|가상 클라이언트를 실행할 스레드 수|
|`-M`|`simple`<br>`extended`<br>`prepared`|일반적인 쿼리 프로토콜<br>확장 쿼리 프로토콜<br>확장 쿼리 프로토콜＋Prepared Statements|
|`-P`|`number`|초단위로 레포트 결과를 표시|
|`-t`|`number`|Transaction 수|
|`-T`|`number`|테스트할 시간 (초)|
>`-t`와 `-T`는 동시에 실행 불가

<br>

### 삭제
```sql
DROP DATABASE pgbench;
```

<br>

### 주의점
데이터베이스만 테스트하기 때문에 연계되어있는 Web 어플리케이션이나 네트워크 등의 다른 환경에 대해서 반영되지 않음

<br>
