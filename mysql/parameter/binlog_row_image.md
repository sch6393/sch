binlog_row_image
===
>ROW 방식으로 Binlog 기록 시 로깅할 컬럼 SET 설정 

### https://dev.mysql.com/doc/refman/8.0/en/replication-options-binary-log.html#sysvar_binlog_row_image

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'binlog_row_image';
/*
+------------------+-------+
| Variable_name    | Value |
+------------------+-------+
| binlog_row_image | FULL  |
+------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
|설정값|설명|
|-|-|
|`FULL`|변경 전, 후 이미지 전체를 기록 (기본값)|
|`MINIMAL`|변경될 행을 식별하기 위해 필요한 최소한의 이전 이미지의 열만 기록|
|`NOBLOB`|행을 식별할 필요가 없거나 `BLOB`나 `TEXT` 같은 열을 제외하고 기록|

* [NDB Cluster](../cluster/ndb/README.md)에서 지원되지 않으므로 NDB 테이블의 로깅에 영향을 주지 않음
* `MINIMAL` 또는 `NOBLOB` 를 사용하는 경우 소스 테이블과 대상 테이블 모두에 대해 이하의 조건이 맞는 경우에만 삭제 및 업데이트가 지정된 테이블에 대해 정상 작동을 보장함
  1. 모든 열은 동일한 순서로 존재해야 하며 각 열은 다른 표의 데이터 유형과 동일한 데이터 유형을 사용해야 함
  1. 테이블에는 동일한 기본 키 정의가 있어야 함

* [binlog_format](../log/binary/README.md) 해당 문서 참조

<br>

### 관련 파라미터
* [sync_binlog](./sync_binlog.md)

<br>
