group_concat_max_len
===
>`GROUP_CONCAT()` 함수에서 허용되는 최대 바이트 길이 설정

### https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_group_concat_max_len

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'group_concat_max_len';
/*
+----------------------+-------+
| Variable_name        | Value |
+----------------------+-------+
| group_concat_max_len | 1024  |
+----------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
* 기본값은 `1024`

<br>
