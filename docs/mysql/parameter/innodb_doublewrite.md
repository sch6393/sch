innodb_doublewrite
===
>Double Write Buffer 사용 여부 설정

### https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_doublewrite

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_doublewrite';
/*
+--------------------+-------+
| Variable_name      | Value |
+--------------------+-------+
| innodb_doublewrite | ON    |
+--------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
* [Double Write Buffer](../double-write-buffer/README.md) 문서 참조

|설정값|설명|
|-|-|
|`ON`|Double Write Buffer 활성화|
|`OFF`|Double Write Buffer 비활성화|
|`DETECT_AND_RECOVER`|`ON`과 동일|
|`DETECT_ONLY`|메타 데이터만 활성화|
>활성화, 비활성화 간의 값 변경은 Dynamic이 아님

* Atomic Write를 지원하는 Fusion-io 장치를 사용할 경우 자동으로 이중 쓰기 버퍼가 비활성화되고 Fusion-io의 Atomic Write를 사용하여 데이터 파일 쓰기가 실행됨. `innodb_doublewrite` 설정은 전역적으로 적용되므로 이중 쓰기 버퍼가 비활성화되면 Fusion-io 하드웨어에 존재하지 않는 파일을 포함한 모든 데이터 파일에 대해 비활성화됨. 이 기능은 Fusion-io 하드웨어에서만 지원되며 Linux의 Fusion-ion NVMFS에서만 활성화됨. 이 기능을 최대한 활용하기 위해서는 [`innodb_flush_method`](./innodb_flush_method.md#관련-내용) 파라미터 값을 `O_DIRECT` 로 설정할 것을 권장

<br>

### 관련 파라미터
* [innodb_doublewrite_batch_size](./innodb_doublewrite_batch_size.md)
* [innodb_doublewrite_dir](./innodb_doublewrite_dir.md)
* [innodb_doublewrite_files](./innodb_doublewrite_files.md)
* [innodb_doublewrite_pages](./innodb_doublewrite_pages.md)

<br>
