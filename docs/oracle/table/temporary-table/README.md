Temporary Table
===

### 설명
세션 활성, 트랜잭션 동안에 임시적으로 데이터를 보관함. 세션이나 트랜잭션이 종료됐을 경우 `TRUNCATE` 됨. PLAN 테이블이 대표적.

<br>

### 확인
```sql
--DURATION : 데이터 유지 기간
SELECT OWNER, TABLE_NAME, TEMPORARY, DURATION FROM ALL_TABLES
WHERE TEMPORARY = 'Y' AND OWNER LIKE 'owner_name';
```

<br>

### 생성 및 삭제
```sql
--GLOBAL : 레코드만 임시, 테이블 정의는 지속
--세션 단위
CREATE GLOBAL TEMPORARY TABLE owner_name.table_name (
   column_name1  data_type
  ...
  ,column_name9  data_type
) ON COMMIT PRESERVE ROWS;

--트랜잭션 단위
CREATE GLOBAL TEMPORARY TABLE owner_name.table_name (
   column_name1  data_type
  ...
  ,column_name9  data_type
) ON COMMIT DELETE ROWS;

--PRIVATE : 레코드, 테이블 정의 모두 임시로 가능
--트랜잭션이 완료된 후 테이블 정의를 지속
CREATE PRIVATE TEMPORARY TABLE ORA$PTT_table_name (
   column_name1  data_type
  ...
  ,column_name9  data_type
) ON COMMIT PRESERVE DEFINITION;

--트랜잭션이 완료된 후 테이블 정의를 삭제
CREATE PRIVATE TEMPORARY TABLE ORA$PTT_table_name (
   column_name1  data_type
  ...
  ,column_name9  data_type
) ON COMMIT DROP DEFINITION;

--삭제 (일반 테이블 삭제하는 것과 같음)
DROP TABLE owner_name.table_name;
```

<br>

### GLOBAL TEMPORARY 특징
1. 임시 데이터이므로 RAC 시스템에서 동기화하지 않음
1. 각 세션간의 데이터 경합이 없으므로 DML LOCK이 없음
1. REDO LOG가 발생하지 않음
1. 인덱스, 뷰, 트리거를 생성할 수 있으나 해당 오브젝트들도 전부 TEMPORARY 로 생성됨

<br>

### PRIVATE TEMPORARY 특징
1. 테이블 이름이 무조건 `ORA$PTT_` 로 시작해야함
1. 컬럼에 대한 기본값을 지정할 수 없음
1. 데이터베이스 링크로 접근할 수 없음
1. 인덱스, 뷰를 생성할 수 없음

<br>

### 참고
* [Table](../../table/README.md)
* [ORA-14451](../../error/14451.md)
* [ORA-32463](../../error/32463.md)

<br>
