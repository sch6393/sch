innodb_flush_neighbors
===
>InnoDB 버퍼 풀에서 동일한 범위의 다른 Dirty 페이지를 플러쉬 여부 설정

### https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_flush_neighbors

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_flush_neighbors';
/*
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| innodb_flush_neighbors | 0     |
+------------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
|설정값|설명|
|-|-|
|`0`|비활성화|
|`1`|동일한 범위의 연속된 Dirty 페이지를 플러쉬|
|`2`|동일한 범위의 Dirty 페이지를 플러쉬|
* HDD에 데이터가 저장되어 있는 경우 인접 페이지를 플러쉬할때 한 번의 작업으로 하는 것이 개별 페이지를 여러 번 작업하는 것 보다 I/O 이점을 가질 수 있음
* SSD의 경우 중요한 요소가 아니기에 쓰기 작업 분산 목적이라면 `0` 으로 설정

<br>
