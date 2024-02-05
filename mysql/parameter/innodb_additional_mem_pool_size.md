innodb_additional_mem_pool_size
===
>데이터 딕셔너리 정보, 내부 데이터 구조를 메모리에 설정 가능한 크기 설정

### MySQL 5.6.3 이후로 Deprecated 되었음.

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_additional_mem_pool_size';
/*
+-------------------------------+----------+
| Variable_name                 | Value    |
+-------------------------------+----------+
| innodb_buffer_pool_chunk_size | 16777216 |
+-------------------------------+----------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
* [innodb_use_sys_malloc](./innodb_use_sys_malloc.md) 가 활성화 되어있다면 사용되지 않음

<br>

### 관련 파라미터
* [innodb_use_sys_malloc](./innodb_use_sys_malloc.md)

<br>
