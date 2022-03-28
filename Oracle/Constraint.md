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
1. [[C] NOT NULL](./Constraint/NotNull.md)
1. [[C] CHECK](./Constraint/Check.md)
1. [[U] UNIQUE](./Constraint/Unique.md)
1. [[P] PRIMARY KEY](./Constraint/PrimaryKey.md)
1. [[R] FOREIGN KEY](./Constraint/ForeignKey.md)
1. [DEFAULT](./Constraint/Default.md)

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
