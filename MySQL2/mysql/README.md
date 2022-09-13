mysql
===

### 기본 형식
```sh
mysql -u USER
```

<br>

### 비밀번호 지정 시
```sh
mysql -u USER -p
# Enter password: PASSWORD

# 비밀번호를 커맨드에 같이 입력시 패스워드는 붙여씀
mysql -u USER -pPASSWORD
```

<br>

### 그 외의 옵션
```sh
# 원격접속을 위한 호스트, 포트 지정
mysql -h HOST -u USER -pPASSWORD -P PORT

# 스키마 지정
mysql -u USER -pPASSWORD -D SCHEMA

# 쿼리를 입력하여 결과 출력 (csv)
query="SELECT column FROM table;"
mysql -h HOST -u USER -pPASSWORD -P PORT -D SCHEMA -e "$query" | tr '\t' ',' > /aaa/bbb/query.csv

```

<br>

### 시작, 정지, 재시작, 상태 확인
|분류|우분투|CentOS 6|CentOS 7|
|-|-|-|-|
|시작|`service mysql start`|`service mysqld start`|`systemctl start mysqld`|
|정지|`service mysql stop`|`service mysqld stop`|`systemctl stop mysqld`|
|재시작|`service mysql restart`|`service mysqld restart`|`systemctl restart mysqld`|
|상태 확인|`service mysql status`|`service mysqld status`|`systemctl status mysqld`|

<br>
