User Mapping
===

### 설명
외부 서버를 사용하는 사용자 맵핑을 정의함. 외부 서버에 있는 자료를 사용하기 위한 그 서버의 사용자 정보와 로컬 서버 사용자 사이의 정보이며 이런 방식으로 (캡슐화) 로컬 사용자는 외부 서버 접속 관련 사용자 설정을 몰라도 동작할 수 있도록 함.

맵핑은 해당 외부 서버 개체의 권한을 가진 유저만 만들 수 있음.

<br>

### 생성
```sql
CREATE USER MAPPING
    FOR user_name
    SERVER server_name
OPTIONS (
        user 'user_name', password 'password'
);
```

<br>

### 수정
```sql
--server_name의 서버에서 user_name이라는 사용자 맵핑의 비밀번호를 변경
ALTER USER MAPPING FOR user_name SERVER server_name
OPTIONS (
    SET password 'change_password'
);
```

<br>

### 삭제
```sql
--server_name의 서버에서 user_name이라는 사용자 맵핑이 있으면 삭제
DROP USER MAPPING IF EXISTS FOR user_name SERVER server_name;
```

<br>

### 관련 항목
* [Server](../server/README.md)

<br>
