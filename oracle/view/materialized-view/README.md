Materialized View
===

### 정의
구체화 뷰라고도 함. 오라클의 Materialized View는 일반 뷰와 달리 데이터를 일정 공간 차지하며 물리적으로 존재하는 뷰.

특정 쿼리가 엄청난 빈도로 사용될 경우 고려할 수 있으며 쿼리 실행, 수행 속도의 향상에 도움을 줄 수 있음.

대용량의 데이터를 처리하는 쿼리에 대해서 쿼리의 결과 만큼 새로운 테이블을 생성해 놓고 자주 사용되는 뷰 결과를 디스크에 저장함으로서 쿼리 속도를 향상시킴

<br>

### 특징
1. Query Rewrite : 사용자가 SQL문을 실행할 때 Materialized View와 마스터 테이블 중 어느 쪽으로 드라이빙 할 것인지 선택하고 선택된 결과에 따라 SQL문을 수정함 (엔터프라이즈 기능)
1. 마스터 테이블이 변경되면 자동으로 갱신됨 (읽기 일관성이 유지)
1. 일반 뷰와는 달리 디스크에 물리적인 공간이 있음
1. Materialized View 작성 시에 인덱스는 최소한 1개는 자동으로 생성됨 (Refresh 방식이 FAST 일때만)
1. 파티션이 가능
1. Dimension 객체를 생성해 좀 더 확장된 관계 연산이 가능

<br>

### 기본 형식
```sql
--1. 뷰에서 데이터를 가져오려는 마스터 테이블에 대해 통계정보가 있어야 함
ANALYZE TABLE owner_name.tablen_name ESTIMATE STATISTICS;

--2. 뷰에서 데이터를 가져오려는 마스터 테이블에 대해 Materialized View Log를 작성 (REFRESH COMPLETE라면 필요 없음)
CREATE MATERIALIZED VIEW LOG ON owner_name.table_name
WITH PRIMARY KEY, ROWID INCLUDING NEW VALUES;

--3. Materialized View 작성
CREATE MATERIALIZED VIEW owner_name.view_name
    TABLESPACE tablespace_name
    BUILD IMMEDIATE       --빌드 모드 설정
    REFRESH FAST          --데이터 갱신 범위 설정
    ON COMMIT             --데이터 갱신 시기 설정
    ENABLE QUERY REWRITE  --쿼리 재작성 설정
    USING INDEX           --인덱스 설정
AS
    SELECT ... FROM owner_name.table_name
;
```

<br>

### 옵션 설명
|예약어|설정값 및 설명|
|-|-|
|`BUILD`|`IMMEDIATE` : Materialized View 생성과 동시에 데이터 생성<br>`DEFERRED` : Materialized View 생성하지만 데이터는 생성하지 않음|
|`REFRESH`|`FAST` : 변경된 데이터만 갱신<br>`COMPLETE` : 전체 데이터 갱신<br>`FORCE` : `FAST` 로 실행한 후 실패한다면 `COMPLETE` 로 실행|
|`ON`|`DEMAND` : 수동으로 리프레시 (`DBMS_MVIEW`)<br>`COMMIT` : 자동으로 리프레시 (커밋할 때 마다)|
|`ENABLED QUERY REWRITE`|사용자가 실행한 SQL을 옵티마이저가 재작성|
|`DISABLED QUERY REWRITE`|사용자가 실행한 SQL을 옵티마이저가 재작성하지 않음|
|`USING`|`INDEX` : 자동으로 인덱스 생성<br>`NO INDEX` : 인덱스를 생성하지 않음|

<br>

### 파티션
```sql
CREATE MATERIALIZED VIEW owner_name.view_name
PARALLEL PARTITION BY LIST (key) (
    PARTITION partition_name1 VALUES (value),
    PARTITION partition_name2 VALUES (value),
    PARTITION partition_name3 VALUES (value),
    ...
)
    TABLESPACE tablespace_name
    BUILD IMMEDIATE       --빌드 모드 설정
    REFRESH FAST          --데이터 갱신 범위 설정
    ON COMMIT             --데이터 갱신 시기 설정
    ENABLE QUERY REWRITE  --쿼리 재작성 설정
    USING INDEX           --인덱스 설정
AS
    SELECT ... FROM owner_name.table_name
;
```
>파티션인 경우 병렬 DML이 가능하기 때문에 리프레시 시간 단축, SQL 수행 시간 단축의 효과가 있음

<br>

### 리프레시
```sql
EXEC DBMS_MVIEW.REFRESH('owner_name.view_name', 'option');
EXEC DBMS_SNAPSHOT.REFRESH('owner_name.view_name', 'option');
/*
Option 설명
c : COMPLETE
f : FAST
*/

--매 1시간마다 리프레시
CREATE MATERIALIZED VIEW owner_name.view_name
    TABLESPACE tablespace_name
    BUILD IMMEDIATE
    REFRESH FAST          --데이터 갱신 범위 설정
    START WITH SYSDATE
    NEXT SYSDATE + (1 / 24)
AS
    SELECT ... FROM owner_name.table_name
;

--하루에 한 번씩 리프레시 하도록 조정 
ALTER MATERIALIZED VIEW owner_name.view_name REFRESH NEXT SYSDATE + 1;
 
--혹여나 다른 계정에서 리프레시를 하고 싶다면 권한이 필요
GRANT ALTER ON owner_name.view_name VIEW TO owner_name_other;
GRANT ALTER ANY MATERIALIZED VIEW TO owner_name_other; --전체 대상
```

<br>

### 확인
* 정합성 (Integrity) 확인
    ```sql
    SELECT * FROM ALL_MVIEWS;
    ```
    |`STALENESS` 컬럼 값|설명|
    |-|-|
    |`FRASH`|정합성 일치|
    |`STALE`|정합성 불일치|
    |`UNUSABLE`|Materialized View 오브젝트만 있고 데이터가 없음|
    |`UNKNOWN`|정합성을 알 수 없음|
    |`UNDEFINED`|원본 테이블 중 1개 이상이 원격지에 있는 경우|

<br>

* 유효성 (Validation) 확인
    ```sql
    SELECT * FROM ALL_MVIEWS; --COMPILE_STATE
    SELECT * FROM ALL_OBJECTS; --STATUS
    ```
    |`COMPILE_STATE` 컬럼 값|`STATUS` 컬럼 값|설명|
    |-|-|-|
    |`VALID`|`VALID`|유효성이 검증됨|
    |`NEED_COMPILE`|`INVALID`|유효성이 검증되지 않음 (DML)|
    |`COMPILATION_ERROR`|`INVALID`|유효성이 틀림 (DDL)|

    `NEED_COMPILE` 이 발생했다면 `ALTER MATERIALIZED VIEW owner_name.view_name COMPILE;` 로 컴파일하여 유효성을 다시 검증 할 수 있으며 이 때 원본 테이블에 기존 Materialized View의 쿼리를 수행 할 수 없을 만큼의 DDL이 된다면 `COMPILE` 후에 `COMPILATION_ERROR` 가 표시됨

    수동 REFRESH (`ON DEMAND`) 일 때는 DML이 발생할때 마다 `NEED_COMPILE` 상태가 되며 그 때마다 `COMPILE` 을 해주지 않으면 유효성이 검증되지 않은 상태가 유지됨. 자동 REFRESH (`ON COMMIT`) 일 때는 `COMPILE` 작업이 필요 없음 

    <br>

    Materialized View는 특정 시점마다 유효성을 체크함 (Revalidation)
    * `REFRESH` : `CONTAINER TABLE` 을 갱신하기 전에 유효성을 확인
    * `QUERY REWRITE` : `QUERY WRITE` 를 하기 전에 유효성이 맞는지 확인

<br>

### 관련문서
* [ORA-12014](../../error/12014.md)

<br>
