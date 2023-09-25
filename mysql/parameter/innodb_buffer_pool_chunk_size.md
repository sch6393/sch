innodb_buffer_pool_chunk_size
===
>InnoDB 버퍼 풀 청크 크기 설정

### https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_buffer_pool_chunk_size

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_buffer_pool_chunk_size';
/*
+-------------------------------+-----------+
| Variable_name                 | Value     |
+-------------------------------+-----------+
| innodb_buffer_pool_chunk_size | 134217728 |
+-------------------------------+-----------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
* 값을 변경하게 되면 버퍼 풀 크기가 자동으로 증가하게 되므로 주의가 필요
* [innodb_buffer_pool_size](./innodb_buffer_pool_size.md) / `innodb_buffer_pool_chunk_size` 가 1000개를 초과하지 않도록 조정이 필요

<br>

### 관련 파라미터
* [innodb_buffer_pool_instances](./innodb_buffer_pool_instances.md)
* [innodb_buffer_pool_size](./innodb_buffer_pool_size.md)

<br>
