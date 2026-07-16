Optimized Writes
===
>https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/rds-optimized-writes.html

### 개요
[이중 쓰기 버퍼](#참고)를 사용하지 않고 한 번만 쓰기 작업을 통해 쓰기 트랜잭션 처리량을 최대 2배까지 높일 수 있음. 스토리지의 경우 AWS의 Nitro 시스템을 사용

<br>

### 활성화 조건
|항목|조건|
|-|-|
|MySQL 버전|`8.0.30` 이상|
|인스턴스|`db.m7g`<br>`db.m6g`<br>`db.m6gd`<br>`db.m6i`<br>`db.m5d`<br>`db.r7g`<br>`db.r6g`<br>`db.r6gd`<br>`db.r6i`<br>`db.r5`<br>`db.r5b`<br>`db.r5d`<br>`db.x2iedn`|

<br>

### 설정값
|파라미터|값|설명|
|-|-|-|
|`rds.optimized_writes`|`AUTO`|데이터베이스에서 지원하는 경우 활성화, 지원하지 않을 경우 비활성화 (기본값)|
|`rds.optimized_writes`|`OFF`|데이터베이스에서 지원하더라도 비활성화|
```sql
# 이하의 쿼리로도 사용 중인지 확인 가능
SHOW VARIABLES LIKE 'innodb_doublewrite';
/*
+--------------------+-------+
| Variable_name      | Value |
+--------------------+-------+
| innodb_doublewrite | ON    |
+--------------------+-------+
*/
```

<br>

### 주의점
1. 해당 옵션에 따른 스냅샷 호환은 되지 않으므로 주의

<br>

### 참고
* [이중 버퍼 쓰기](../../../../mysql/double-write-buffer/README.md)
* [Nitro 시스템](https://aws.amazon.com/ec2/nitro/?nc1=h_ls)

<br>
