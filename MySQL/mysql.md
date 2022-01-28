mysql
===

### 1. 기본 형식
```sh
mysql -u USER
```

<br>

### 2. 비밀번호 지정 시
```sh
mysql -u USER -p
# Enter password: PASSWORD

# 비밀번호를 커맨드에 같이 입력시 패스워드는 붙여씀
mysql -u USER -pPASSWORD
```

<br>

### 3. 그 외의 옵션
```sh
# 원격접속을 위한 호스트, 포트 지정
mysql -u USER -pPASSWORD -h HOST -P PORT

# 스키마 지정
mysql -u USER -pPASSWORD -D SCHEMA

# 쿼리를 입력하여 결과 출력 (csv)
query="SELECT column FROM table;"
mysql -u USER -pPASSWORD -h HOST -P PORT -D SCHEMA -e "$query" | tr '\t' ',' > /aaa/bbb/query.csv

```

<br>

### 

<br>
