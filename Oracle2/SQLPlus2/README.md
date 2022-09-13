SQL Plus
===

### SQL Plus 옵션
```sh
# 명령어 표시 유무
SET ECHO OFF

# 데이터 조회 결과에서 헤더 표시 유무
SET HEAD OFF

# 데이터 조회 결과 표시 유무
SET TERM ON

# 데이터 조회 결과에 대한 건수, 시간 표시 유무
SET FEEDBACK OFF

# 라인 끝 공백 제거 유무
SET TRIMSPOOL ON

# PL/SQL 결과 표시 유무
SET SERVEROUTPUT ON

# Default : 14, 만약 Default 값이라면 14 라인마다 1 라인씩 공백 생성
SET PAGES 0

# 레코드 길이를 지정
SET LINES 300

# csv 파일 형식
SET MARKUP CSV ON

# SQL Plus 연결 정보 미표시
sqlplus -S
```

<br>

### .sql 파일 실행
```sh
sqlplus id/pw @/AAA/query.sql > result.txt
```

<br>

### SYSDBA 접속
```sql
sqlplus / as sysdba
```

<br>

### 원격 데이터베이스 접속
```sql
sqlplus id/pw@'tns_name'
```

<br>

### 아카이브 모드 확인
```sql
ARCHIVE LOG LIST;

/*
Database log mode              No Archive Mode
Automatic archival             Disabled
Archive destination            /AAA/BBB/archive
Oldest online log sequence     00000
Current log sequence           00000
*/
```

<br>

### 시작, 정지, 모드 진입
```sql
--시작
startup

--마운트하지 않고 시작
startup nomount

--정지
shutdown

--강제 정지
shutdown immediate
```

<br>
