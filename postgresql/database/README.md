Database
===

### 구조
MySQL에서는 논리 Database를 Schema와 같은 의미로 사용되지만 PostgreSQL에서는 Database와 Schema 두 가지 개념 모두 사용되며 Database는 Schema의 상위 개념임. 테이블의 집합을 Schema로 표현하며 Schema는 하나의 Database를 논리적으로 나누는 개념이므로 MySQL에서의 논리 Database는 PostgreSQL에서의 Schema라고 봐야함

이런 차이점 때문에 PostgreSQL에서는 같은 DB 인스턴스 안에 있다고 해도 서로 다른 Database에 있는 테이블 간의 JOIN 연산이 불가능함 (서로 다른 Schema는 가능)

<br>

### 확인
```sql
--Shell
\l+

--Query
SELECT * FROM PG_DATABASE;
```

<br>

### 생성
```sql
CREATE DATABASE database_name;

--해당 유저에 종속
CREATE DATABASE database_name WITH OWNER user_name;
```

<br>

### 삭제
```sql
DROP DATABASE database_name;
```

<br>
