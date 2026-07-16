Query Rewrite
===

### 정의
SQL을 수행 하기 전에 옵티마이저가 SQL에 대한 실행계획을 생성하고 동시에  Query Rewrite한 SQL과 실행계획을 생성함. 옵티마이저는 둘 중 Cost 가 낮은 실행 계획을 선택하여 실행함 

<br>

### [Materialized View](../view/materialized-view/README.md) 에서 아래의 쿼리들은 Query Rewrite가 동작함
1. SELECT
1. CREATE TABLE ... AS SELECT
1. INSERT INTO ... SELECT
1. DML Statements에 대한 서브쿼리 (INSERT, UPDATE, DELETE)
1. Set Operators를 위한 서브쿼리 (UNION, UNION ALL, INTERSECT, MINUS)

<br>

### 조건
1. Query Rewrite 대상 테이블에 대한 통계정보가 있어야 함. 당연히 최신의 통계정보를 가지고 있는 게 좋음 ([Analyze](../analyze/README.md) Table)
1. Materialized View에서 `Enable Query Rewrite` 옵션이 있어야 함
1. `query_rewrite_enabled` 파라미터가 `TRUE` 거나 `FORCE` 이어야 함 (기본값은 `TRUE` 이며 `ALTER SESSION SET` 으로 해당 세션만 변경해서 사용도 가능)
1. [CBO (Cost Based Optimizer)](../cbo-rbo/README.md) Type의 옵티마이저를 사용해야 함
1. `OPTIMIZER_FEATURES_ENABLE` 파라미터가 `8.1.6` 이상 이어야 함

<br>

### 제약사항
1. Materialized View에서 Query Rewrite에 대해 Flashback이 불가능

<br>

### HINT

|HINT NAME|DESCRIPTION|
|-|-|
|`NOREWRITE`|Query Rewrite를 사용하지 않음|
|`REWRITE (materialized_view_name)`|Query Rewrite를 강제함|

<br>

### `QUERY_REWRITE_INTEGRITY` PARAMETER에 대한 정보

|VALUE|DESCRIPTION|
|-|-|
|`ENFORCED`|기본값이며 FRESH 상태의 Materialized View이어야 하고 제약조건 (Constraints) 이 유효한 상태여야만 Query Rewrite를 실행|
|`TRUSTED`|VIEW나 PREBUILT TABLE을 기반으로 만들어진 Materialized View의 데이터, 그리고 Dimension에 기술된 관계와 제약조건 (Constraints) 들을 신뢰함. 그래서 Dimension의 관계나 제약조건이 정확하지 않으면 잘못된 데이터를 생성할 가능성이 있음|
|`STALE_TOLERATED`|STALE 데이터를 가지고 있어도 옵티마이저가 유효하다고 판단 후 Query Rewrite를 실행. 그래서 STALE 데이터로 인해 잘못된 데이터를 생성할 가능성이 있음 (`DBA_MVIEWS`의 `STALENESS` 컬럼의 값이 STALE 이라고 표시되어도 VALID로 판단)|

<br>

### Query Rewrite의 동작 여부
`DBMS_MVIEW.EXPLAIN_REWRITE` 패키지를 사용해 분석이 가능함. 해당 패키지를 수행한 결과는 `REWRITE_TABLE` 에 저장됨

1. `REWRITE_TABLE` 작성
    ```sh
    # $ORACLE_HOME/rdbms/admin
    utlxrw.sql
    ```

1. `DBMS_MVIEW.EXPLAIN_REWRITE` 패키지 실행
    ```sql
    DECLEAR
        sql_text VARCHAR2(200) := 'SELECT ... ';
    BEGIN
        dbms_mview.explain_rewrite (sql_text, 'materialized_view_name', 'statement_id')
    END;
    ```

1. `REWRITE_TABLE` 조회
    ```sql
    SELECT message FROM REWRITE_TABLE WHERE statement_id = 'statement_id' ORDER BY sequence;
    /*
    QSM-01001: query rewrite not enabled
    */
    ```

<br>

### Query Rewrite Methods의 디멘션과 제약조건의 필수 여부

|Query Rewrite Type|Dimensions|Primary Key, Foreign Key, Not Null Constraints|
|-|-|-|
|Matching SQL Text|필수 아님|필수 아님|
|Join Back|필수 또는 필수 아님|필수|
|Aggregate Computability|필수 아님|필수 아님|
|Aggregate Rollup|필수 아님|필수 아님|
|Rollup Using a Dimension|필수|필수 아님|
|Filtering the Data|필수 아님|필수 아님|
|PCT Rewrite|필수 아님|필수 아님|
|Multiple Materialized View|필수 아님|필수 아님|

<br>
