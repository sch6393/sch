innodb_use_sys_malloc
===
>InnoDB의 메모리 할당 방법 설정

### MySQL 5.6.3 이후로 Deprecated 되었음.

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_use_sys_malloc';
/*
+-----------------------+-------+
| Variable_name         | Value |
+-----------------------+-------+
| innodb_use_sys_malloc | ON    |
+-----------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
|설정값|설정값 (숫자)|설명|
|-|-|-|
|`ON`|`0`|운영 체제의 메모리 할당을 사용|
|`OFF`|`1`|엔진 자체의 메모리 할당을 사용|

<br>

### 관련 파라미터
* [innodb_additional_mem_pool_size](./innodb_additional_mem_pool_size.md)

<br>
