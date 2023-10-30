innodb_doublewrite_dir
===
>Double Write Buffer 파일 생성 디렉토리 설정

### https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_doublewrite_dir

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_doublewrite_dir';
/*
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| innodb_doublewrite_dir |       |
+------------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
* [Double Write Buffer](../double-write-buffer/README.md) 문서 참조
* 스키마 이름과의 충돌을 방지하기 위해 파일 맨 앞에 `#` 기호가 자동으로 생성됨

<br>

### 관련 파라미터
* [innodb_doublewrite](./innodb_doublewrite.md)
* [innodb_doublewrite_batch_size](./innodb_doublewrite_batch_size.md)
* [innodb_doublewrite_files](./innodb_doublewrite_files.md)
* [innodb_doublewrite_pages](./innodb_doublewrite_pages.md)

<br>
