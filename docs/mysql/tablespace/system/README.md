시스템 테이블스페이스
===
>https://dev.mysql.com/doc/refman/8.0/en/innodb-system-tablespace.html

### 내용

파일 단위 테이블이나 일반 테이블스페이스가 아닌 시스템 테이블스페이스에 테이블을 생성한 경우 테이블 및 인덱스 데이터가 포함될 수 있음

<br>

### 크기 늘리기
```sql
# 파라미터를 설정해서 늘리기 가능
innodb_data_file_path=ibdata1:10M:autoextend
```
>크기를 줄이는 것은 인스턴스를 재생성하는 것 외에는 불가능함

<br>

### 관련 파라미터
* [innodb_autoextend_increment](../../parameter/innodb_autoextend_increment.md)
* [innodb_data_file_path](../../parameter/innodb_data_file_path.md)

<br>
