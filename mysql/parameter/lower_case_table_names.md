lower_case_table_names
===
>테이블 이름 및 데이터베이스 이름 대소문자 구분 여부

### https://dev.mysql.com/doc/refman/8.0/en/identifier-case-sensitivity.html

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'lower_case_table_names';
/*
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| lower_case_table_names | 1     |
+------------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
|설정값|설명|
|-|-|
|`0`|테이블 이름 및 데이터베이스 이름을 지정된 대소문자를 사용해 디스크에 저장, 이름 비교는 대소문자를 구분함 (Windows처럼 대소문자를 구분하지 않는 파일 시스템을 가진 OS에서는 0으로 설정하지 말 것)|
|`1`|테이블 이름 및 데이터베이스 이름을 소문자로 디스크에 저장, 이름 비교는 대소문자를 구분하지 않음|
|`2`|테이블 이름 및 데이터베이스 이름을 지정된 대소문자를 사용해 디스크에 저장, MySQL에서 조회 시에 소문자로 변환, 이름 비교는 대소문자를 구분하지 않음|

<br>
