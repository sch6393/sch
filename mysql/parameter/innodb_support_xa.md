innodb_support_xa
===
>XA 트랜잭션에서 2단계 커밋에 추가로 디스크 플러쉬를 발생

### https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html#sysvar_innodb_support_xa

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_support_xa';
/*
+-------------------+-------+
| Variable_name     | Value |
+-------------------+-------+
| innodb_support_xa | ON    |
+-------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
* `5.7.10` 이후로 Deprecated 되고 강제 활성화되었음

<br>

### 관련 파라미터
* [sync_binlog](./sync_binlog.md)

<br>
