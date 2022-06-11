Constraint
===

### 정의
* 데이터의 무결성을 지키기 위해 제한된 조건 (제약조건)

<br>

### 확인
```sql
SELECT * FROM ALL_CONSTRAINTS WHERE TABLE_NAME = 'table_name';
```

<br>

### 종류
1. [C] NOT NULL
    * NULL값을 허용하지 않음
1. [C] CHECK
    * 조건에 벗어나는 값은 허용하지 않음
      >ex) CHECK(column_name >= 10 AND column_name <= 100)
1. [U] UNIQUE
    * 중복값을 허용하지 않음
1. [P] PRIMARY KEY
    * UNIQUE + NOT NULL
    * 테이블당 한개만 정의 가능하며 자동 INDEX가 생성됨
1. [R] FOREIGN KEY
    * PK에 있는 값만 허용
    * `ON DELETE CASCADE` : 상위 테이블의 데이터가 없어지면 같이 없어짐
    * `ON DELETE SET NULL` : 상위 테이블의 데이터가 없어지면 NULL로 변경됨
1. DEFAULT
    * 기본 값을 지정함

<br>

### 생성
* [CREATE TABLE](./Table.md#table-생성) 할 때 정의하여 생성

<br>

### 추가
```sql
--Constraint 이름을 따로 지정
ALTER TABLE owner_name.table_name ADD CONSTRAINT constraint_name constraint_type(column_name);

--Constraint 이름을 따로 지정하지 않음
ALTER TABLE owner_name.table_name ADD constraint_type(column_name);
```

<br>

### 변경
```sql
ALTER TABLE owner_name.table_name MODIFY column_name constraint_type;
```

<br>

### 삭제
```sql
ALTER TABLE owner_name.table_name DROP CONSTRAINT constraint_name;
```

<br>
