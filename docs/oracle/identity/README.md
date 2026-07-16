Identity
===

### 설명 
MySQL의 AUTO_INCREMENT처럼 사용 가능함

<br>

### 테이블 생성
```sql
CREATE TABLE owner_name.table_name (
    id          NUMBER GENERATED ALWAYS AS IDENTITY,
    column_name VARCHAR2(50)
);
```

<br>

### 데이터 삽입
```sql
--MySQL의 AUTO_INCREMENT처럼 시퀀스 증가 컬럼을 제외한 나머지 컬럼에 데이터를 넣으면 됨
INSERT INTO onwer_name.table_name(column_name) VALUES ('value');
```

<br>

### IDENTITY 값 초기화
```sql
ALTER TABLE onwer_name.table_name MODIFY(id GENERATED AS IDENTITY (START WITH 1));
```

<br>

### 참고 문서
* [Sequence로 자동 증가](../sequence/README.md#mysql의-auto_increment처럼-사용)

<br>
