innodb_buffer_pool_instances
===
>InnoDB 버퍼 풀에 대한 인스턴스 개수 설정

### https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_buffer_pool_instances

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_buffer_pool_instances';
/*
+------------------------------+-------+
| Variable_name                | Value |
+------------------------------+-------+
| innodb_buffer_pool_instances | 8     |
+------------------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
* 버퍼 풀을 분할하기 위함이며 캐시된 페이지에 대한 읽기 쓰기 작업에서 동시성을 향상시킬 수 있음
* 해싱으로 여러 인스턴스 중 랜덤하게 할당됨

<br>

### 관련 파라미터
* [innodb_buffer_pool_size](./innodb_buffer_pool_size.md)

<br>
