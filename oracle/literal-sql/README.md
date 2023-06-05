Literal SQL
===
>개발 쪽에서는 Dynamic SQL 이라고도 함

### 기본 형식
```sql
--1
SELECT * FROM owner_name.table_name = WHERE column_name = 'A';

--2
SELECT * FROM owner_name.table_name = WHERE column_name = 'B';

/*
오라클에서는 1번과 2번을 다른 SQL로 인식하여 파싱함
만약 저런 쿼리들이 많다면 그 갯수 만큼 파싱 정보를 생성하기 때문에 Shared Pool 공간에 문제가 생길 수 있음
*/

--Variable
SELECT * FROM owner_name.table_name = WHERE column_name = :Variable;

/*
위와 같은 형태로 변경하면 파싱 정보 하나만 생성하여 변수로 대체를 할 수 있음
*/
```

<br>

### Literal SQL 정보
```sql
--SQL 목록
SELECT * FROM V$SQLAREA
WHERE
    EXECUTIONS = 1 AND    --실행 횟수
    PARSING_SCHEMA_NAME IN ('user_name')
;

--Literal SQL 비율
SELECT A.CNT AS TOTAL, B.CNT AS LITERAL, ROUND(B.CNT / A.CNT * 100, 2) AS PERCENT
FROM (
	SELECT COUNT(1) AS CNT FROM V$SQLAREA WHERE PARSING_SCHEMA_NAME IN ('user_name')
) A, (
	SELECT COUNT(1) AS CNT FROM V$SQLAREA WHERE EXECUTIONS = 1 AND PARSING_SCHEMA_NAME IN ('user_name')
) B
;
/*
TOTAL	LITERAL	PERCENT
15	12	80
*/
```

<br>

### 관련 에러
* [ORA-04031](../error/04031.md)

<br>
