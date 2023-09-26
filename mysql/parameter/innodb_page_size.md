innodb_page_size
===
>InnoDB 테이블 스페이스의 페이지 크기를 지정

### https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_page_size

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_page_size';
/*
+------------------+-------+
| Variable_name    | Value |
+------------------+-------+
| innodb_page_size | 16384 |
+------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
* MySQL 인스턴스 초기화 전에 구성할 수 있으며 구성 후엔 변경 불가능
* 5.7부터 32KB, 64KB를 지원함
* Extent 크기는 32KB인 경우 2MB, 64KB인 경우 4MB 
* 32KB, 64KB인 경우 `ROW_FORMAT = COMPRESSED` 는 사용할 수 없음
* 테이블 스캔이나 DML과 관련된 조작이나 조회에 적합

<br>
