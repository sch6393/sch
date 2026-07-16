innodb_buffer_pool_size
===
>InnoDB 버퍼 풀 크기 설정

### https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_buffer_pool_size

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_buffer_pool_size';
/*
+-------------------------+-------------+
| Variable_name           | Value       |
+-------------------------+-------------+
| innodb_buffer_pool_size | 51539607552 |
+-------------------------+-------------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
* 전용 데이터베이스 서버라면 시스템 물리 메모리 크기의 80% 까지 설정 가능 (설정된 버퍼 풀 크기보다 10% 더 증가됨)
* 청크 단위로 작업을 수행하는데 [innodb_buffer_pool_chunk_size](./innodb_buffer_pool_chunk_size.md) 에 설정할 수 있음
* 버퍼 풀 크기는 [innodb_buffer_pool_chunk_size](./innodb_buffer_pool_chunk_size.md) * [innodb_buffer_pool_instances](./innodb_buffer_pool_instances.md) 과 같거나 배수여야 함
* 만약 같지 않거나 배수가 아니라면 그에 맞게 자동으로 조정됨

<br>

### 관련 파라미터
* [innodb_buffer_pool_chunk_size](./innodb_buffer_pool_chunk_size.md)
* [innodb_buffer_pool_instances](./innodb_buffer_pool_instances.md)

<br>
