Sequence
===

### 정의
```sql
--생성
CREATE SEQUENCE seq_name START WITH 1;

--초기화
SELECT setval('seq_name', 1);

--현재 값
SELECT currval('seq_name');

--다음 값
SELECT nextval('seq_name');

--설정한 값부터 재시작
ALTER SEQUENCE seq_name RESTART WITH 1;

--시퀸스 정보 확인 (직접 건들면 안 됨)
SELECT * FROM seq_name;

-- 시퀀스 삭제
DROP SEQUENCE seq_name;
```

<br>

### 특징
1. 데이터베이스 전체에서 공유가 가능 (여러 테이블에서 동일한 시퀀스에 접근이 가능하다는 말) ➞ 테이블의 특정 컬럼에 일련번호를 생성하거나, 여러 테이블에 걸쳐 일련번호를 생성하는 등 다양한 용도로 사용
1. Serial과 다르게 시퀀스에 규칙 지정이 가능

<br>

### 옵션
|옵션|설명|
|-|-|
|`TEMPORARY`|현재 세션에서만 사용 (세션 종료시 삭제)|
|`AS type_name`|`SMALLINT`, `INTEGER`, `BIGINT`를 선택할 수 있으며 기본 값은 `BIGINT`|
|`INCREMENT BY`|양수면 증가, 음수면 감소|
|`MINVALUE`|최소값 설정|
|`MAXVALUE`|최대값 설정|
|`CACHE`|메모리에 설정한 크기 만큼을 올려둠|
|`CYCLE / NO CYCLE`|순환 여부 설정|
|`OWNED BY / OWNED BY NONE`|해당 컬럼과 시퀀스의 의존관계 생성 여부 (테이블과 시퀀스의 소유자가 같아야 함). 테이블, 컬럼이 삭제되면 시퀀스는 자동 삭제됨. 기본 값은 `OWNED BY NONE`|

<br>

### 참고
* [Serial](../serial/README.md)

<br>
