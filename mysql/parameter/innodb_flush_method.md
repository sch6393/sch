innodb_flush_method
===
>InnoDB 데이터 파일 및 로그 파일에 데이터 플러쉬하는 방법 설정

### https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_flush_method

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_flush_method';
/*
+---------------------+-------+
| Variable_name       | Value |
+---------------------+-------+
| innodb_flush_method |       |
+---------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
|설정값|설정값 (숫자)|설명|
|-|-|-|
|`fsync`|`0`|시스템 호출인 `fsync()` 를 사용하여 데이터, 로그 파일을 플러쉬|
|`O_DSYNC`|`1`|데이터 파일은 `fsync()` , 로그 파일은 `O_DSYNC` 로 플러쉬함|
|`littlesync`|`2`|내부 성능 테스트용|
|`nosync`|`3`|내부 성능 테스트용|
|`O_DIRECT`|`4`|`O_DIRECT` 로 데이터 파일을 열고 `fsync()` 로 데이터, 로그 파일을 플러쉬 (일부 리눅스나 FreeBSD, Solaris에서 사용 가능하며 Solaris의 경우 `directio()`)|
|`O_DIRECT_NO_FSYNC`|`4`|`O_DIRECT` 방식과 동일하나 쓰기 작업 후 `fsync()` 를 호출하지 않음|
* 유닉스 계열 시스템에서는 `fsync`, 윈도우에서는 버퍼링 되지 않음
* 숫자 설정은 8.0 이상에서만 가능
* 8.0.14 이전 버전에서는 XFS나 EXT4와 같은 파일 시스템에서 메타데이터 변경 내용을 동기화하는데 `fsync()` 시스템 호출이 적합하지 않음. `fsync()` 이 필요한지 확실치 않는 경우 `O_DIRECT` 를 사용할 것

<br>

### 관련 파라미터
* [innodb_buffer_pool_size](./innodb_buffer_pool_size.md)

<br>