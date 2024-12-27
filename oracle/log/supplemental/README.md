Supplemeltal Log
===

### 정의
Redo 로그는 DML이 발생하는 경우 오직 변경된 컬럼의 데이터에 대해서만 Undo (변경 전 데이터) 와 Redo (변경 후 데이터) 정보를 기록함 (Physiological Logging). Supplemental Logging (Default : Disabled) 은 DML 발생시 Redo 로그에 추가적인 데이터를 더 기록함. Redo 로그는 기본적으로 Instance Recovery 혹은 Media Recovery를 위해서 사용하는데 이러한 복구 동작을 수행하는 과정에서 Supplemental Logging 기능은 사실상 필요가 없음.

하지만 Redo 로그의 ROWID 기록만으로 로우를 구분할 수 있는가 하면 얘기가 달라짐. 일반적인 UPDATE의 경우는 괜찮으나 파티션 UPDATE인 경우 ROWID 변경 가능성이 있고 DELETE, INSERT 작업, 또는 Row Movement가 가능할 경우에도 ROWID가 변경됨. 이는 CDC에서 해당 로우가 복제 됐을 때 원본과 복제의 ROWID가 다르다는 것을 의미함

이러한 이유로 인해 LogMiner 같은 DBMS 복제 솔루션들은 Supplemental Logging을 사용하여 변경된 로우의 키 컬럼 (로우를 식별할 수 있는 컬럼으로 PK나 UK) 의 논리적인 값을 얻기 위해 사용됨. Supplemental Logging을 수행하면 변경된 로우의 모든 컬럼에 대한 Undo 정보를 남기는게 아니고 기본적으로 변경된 데이터와 사용자가 Supplemental Logging 생성 시 지정한 컬럼들에 대해서만 Before 이미지가 남게됨 (물론 Supplemental Logging을 전체 컬럼에 대해서 모두 생성할 수 있지만 Redo 로그의 크기가 커지고 IO 성능에 영향을 줄 수 있음)

<br>

### 확인
```sql
SELECT SUPPLEMENTAL_LOG_DATA_MIN FROM V$DATABASE;
SELECT SUPPLEMENTAL_LOG_DATA_ALL FROM V$DATABASE;

SELECT * FROM DBA_SUPPLEMENTAL_LOGGING;
SELECT * FROM DBA_LOG_GROUPS;
```

<br>

### 활성화
```sql
--DB 단위로 적용
--DB 전체 테이블에 Minimal로 로깅 설정
ALTER DATABASE ADD SUPPLEMENTAL LOG DATA;

--DB 전체 테이블의 전체 컬럼에 로깅 설정
ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;

--DB 전체 테이블의 PK 구성 컬럼에 로깅 설정
ALTER DATABASE ADD SUPPLEMENTAL LOG DATA (PRIMARY KEY) COLUMNS;

--TABLE 단위로 적용
--테이블의 전체 컬럼에 로깅 설정
ALTER TABLE owner_name.table_name ADD SUPPLEMENTAL LOG DATA (ALL) COLUMNS;

--테이블의 PK 구성 컬럼에 로깅 설정
ALTER TABLE owner_name.table_name ADD SUPPLEMENTAL LOG DATA (PRIMARY KEY) COLUMNS;

--테이블의 특정 컬럼에 로깅 설정
ALTER TABLE owner_name.table_name ADD SUPPLEMENTAL LOG DATA (column_name1, column_name2, ...);
```

<br>

### 비활성화
```sql
--DB 단위로 적용
--DB 전체 테이블의 전체 컬럼에 로깅 해제
ALTER DATABASE ADD SUPPLEMENTAL DROP DATA (ALL) COLUMNS;

--DB 전체 테이블의 PK 구성 컬럼에 로깅 해제
ALTER DATABASE ADD SUPPLEMENTAL DROP DATA (PRIMARY KEY) COLUMNS;

--TABLE 단위로 적용
--테이블의 전체 컬럼에 로깅 해제
ALTER TABLE owner_name.table_name DROP SUPPLEMENTAL LOG DATA (ALL) COLUMNS;

--테이블의 PK 구성 컬럼에 로깅 해제
ALTER TABLE owner_name.table_name DROP SUPPLEMENTAL LOG DATA (PRIMARY KEY) COLUMNS;

--테이블의 특정 컬럼에 로깅 해제
ALTER TABLE owner_name.table_name DROP SUPPLEMENTAL LOG DATA (column_name1, column_name2, ...);
```

<br>

### 주의점
`CLOB`, `BLOB`, `LONG`, `LONG RAW`, `ADT` 데이터 타입에 대해서는 로깅 데이터가 생성되지 않음

<br>
