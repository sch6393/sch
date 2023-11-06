log_bin_trust_function_creators
===
>함수 생성 시 제약처리 설정

### https://dev.mysql.com/doc/refman/8.0/en/replication-options-binary-log.html#sysvar_log_bin_trust_function_creators

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'log_bin_trust_function_creators';
/*
+---------------------------------+-------+
| Variable_name                   | Value |
+---------------------------------+-------+
| log_bin_trust_function_creators | ON    |
+---------------------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
|설정값|설명|
|-|-|
|`0`|`CREATE RUUTINE` 또는 `ALTER RUUTINE` 권한 외에 SUPER 권한이 없는 유저는 저장된 함수, 트리거를 만들거나 변경할 수 없음. 또한 함수나 트리거를 `DELICATIC`, `READS SQL DATA`, `NO SQL` 로만 선언 가능|
|`1`|`0`으로 설정했을 때의 제한이 적용되지 않게됨|

* [binary-log](../log/binary/README.md)가 활성화 됐을 때 적용됨
* [Error 1418](../error/1418.md) 문서 참조

<br>
