Serial
===

### 기본 구조
```sql
CREATE TABLE table_name (
    seq SERIAL NOT NULL PRIMARY KEY,
    column_name1 data_type,
    column_name2 data_type,
    column_name3 data_type,
    ...
);
```

<br>

### 특징
1. Serial 타입을 포함한 테이블을 생성하면 Sequence도 생성됨
1. 결국 Serial도 Sequence로 동작하기 때문에 `nextval()` 같은 함수로 호출이 가능

    >각 시퀀스와 관련된 함수에 대한 설명은 [Sequence](../sequence/README.md) 를 확인

<br>

### SQL
```sql
--인서트 시에 Serial 생략 가능 
INSERT INTO table_name(column_name1, column_name2, ...) VALUES ('data1', 'data2', ...);

--DEFAULT로도 가능
INSERT INTO table_name(seq, column_name1, column_name2, ...) VALUES (DEFAULT, 'data1', 'data2', ...);

--RETURNING column_name을 통해서 응답 값을 받을 수 있음
INSERT INTO table_name(column_name1, column_name2, ...) VALUES ('data1', 'data2', ...) RETURNING seq;

💡 아래의 SQL문을 통해 마지막 SERIAL 타입의 다음 값을 확인합니다.
select nextval(pg_get_serial_sequence('public.tb_user', 'user_sq'));

```

<br>

### 종류
|데이터 타입|Bytes|범위|설명|
|-|-|-|-|
|`SMALLSERIAL` (`SERIAL2`)|2 Bytes|1 ~ 32767|16비트|
|`SERIAL` (`SERIAL4`)|4 Bytes|1 ~ 2147483647|32비트|
|`BIGSERIAL` (`SERIAL8`)|8 Bytes|1 ~ 9223372036854775807|64비트|

<br>

### Serial 타입, DEFAULT 속성 차이
|항목|Serial|DEFAULT|
|-|-|-|
|동작|자동적으로 증가하는 정수를 생성|특정 컬럼에 대한 기본 값 설정|
|증가 설정|자동적으로 증가|자동적으로 증가하는 기능 없음|
|가능한 데이터 타입|정수|어떤 데이터 타입이라도 가능|

<br>

### 참고
* [Sequence](../sequence/README.md)

<br>
