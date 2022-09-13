UNDO Tablespace
===

### 확인 
```sql
SHOW PARAMETER UNDO;
/*
NAME              TYPE    VALUE   
----------------- ------- ------- 
temp_undo_enabled boolean FALSE   
undo_management   string  AUTO          관리 방식 (AUTO/MANUAL)
undo_retention    integer 900           데이터 유지 시간 (기본 값 : 900) [단위 : 초]
undo_tablespace   string  UNDO_T1       테이블스페이스 이름
*/
```

<br>

### 크기 변경
```sql
ALTER TABLESPACE undo_tablespace_name RESIZE 8G;
```
>[ORA-03297: file contains used data beyond requested RESIZE value](../Error/03297.md) 에러가 발생한다면 아래의 [교체 항목](#undo-테이블스페이스-교체-순서)을 참조

<br>

### 삭제
```sql
DROP TABLESPACE undo_tablespace_name INCLUDING CONTENTS AND DATAFILES;
```

<br>

### UNDO 테이블스페이스 교체 순서
```sql
--1. 변경할 UNDO 테이블스페이스 생성
CREATE UNDO TABLESPACE undo_tablespace_name_new DATAFILE SIZE size AUTOEXTEND ON MAXSIZE size;

--2. 변경
ALTER SYSTEM SET UNDO_TABLESPACE = 'undo_tablespace_name_new';

--3. UNDO 테이블스페이스 변경 확인
SHOW PARAMETER UNDO_TABLESPACE;

--4. 기존의 UNDO 테이블스페이스 제거
DROP TABLESPACE undo_tablespace_name_old INCLUDING CONTENTS AND DATAFILES;
```

<br>

### Temporary UNDO에 대한 설명
* 12c부터 추가된 기능이며 Temporary Objects (Global Temporary Table (GTT), Subquery Factoring, The WITH Clause) 용 UNDO Segment를 UNDO Tablespace가 아닌 Temporary Tablespace에 저장하는 기능
* 11g까지의 Global Temporary Table에 할당되는 UNDO Segment는 일반 테이블과 마찬가지로 UNDO Tablespace에 저장하는데 이는 몇가지 문제점을 가짐
  1. UNDO Segment를 보호하기 위한 redo log가 발생 (성능 하락)
  1. Retention Period를 충족시키기 위해 UNDO Tablespace의 크기가 커짐
* Temporary UNDO는 redo log를 발생시키지 않으므로 성능 면에서 많은 이득이 있음 (GTT는 복구할 필요가 없음) 시스템 또는 세션 파라미터 `TEMP_UNDO_ENABLED = TRUE` or `FALSE` 로 제어하며 기본값은 `FALSE`
* 세션에서 트랜잭션이 시작되기 전에 지정해야 하며 트랜잭션 진행 중에 지정하면 무시됨
* Temporary UNDO 관련 Statistics 정보들을 조회할 수 있는 V$TEMPUNDOSTAT 뷰가 있으니 참고

<br>
