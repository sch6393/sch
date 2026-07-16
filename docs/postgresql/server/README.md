Server
===

### 설명
외부 자료를 접근하기 위해 사용하는 Foreign Data Wrapper 의 접속 정보를 만들기 위함. (캡슐화) 그래서 해당 접속에 필요한 사용자와 로컬 데이터베이스 사용자를 매핑하는 작업이 필요.

서버 이름은 데이터베이스 내에서 유일한 값이여야 하며 외부 서버를 만들려면 사용자가 Foreign Data Wrapper 에 대해 `USAGE` 권한이 있어야 함.

<br>

### 생성
```sql
CREATE SERVER server_name FOREIGN DATA WRAPPER postgres_fdw
OPTIONS (
    host 'host_name',
    dbname 'database_name',
    port '5432'
);
```

<br>

### 수정
```sql
--server_name 서버의 접속 정보 변경
ALTER SERVER server_name OPTIONS (host 'change_host_name', dbname 'change_database_name');

--server_name 서버의 버전과 host 옵션 값 변경
ALTER SERVER server_name VERSION 'xx.x' OPTIONS (SET host 'change_host_name');
```

<br>

### 삭제
```sql
DROP SERVER IF EXISTS server_name;
```

<br>

### 참고
dblink 모듈을 사용할 시 dblink_connect 함수의 파라미터를 외부 서버 이름으로 사용할 수 있음. (해당 서버에 대해서 사용자가 `USAGE` 권한을 가지고 있어야 함)

<br>

### 관련 문서
* [User Mapping](../user-mapping/README.md)

<br>
