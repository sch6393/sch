Sequence
===

### 생성
```sql
CREATE SEQUENCE owner_name.seq_name;
```

<br>

### 옵션 기본 값 및 설명
|Name|Value|Description|
|-|-|-|
|MIN_VALUE|1|최소 값|
|MAX_VALUE|9999999999999999999999999999|최대 값|
|INCREMENT_BY|1|증가하는 숫자|
|CYCLE_FLAG|N|순환 여부|
|ORDER_FLAG|N|순서대로 생성 여부<br>Y : 시퀀스 값을 건너뛸 수 없음 (RAC 구성)<br>N : 시퀀스 값을 건너뛸 수 있음|
|CACHE_SIZE|20|메모리에 할당해 놓을 양|
|LAST_NUMBER|1|현재 값|
|SCALE_FLAG|N|확장 가능 여부<br>Y : 6자리 숫자가 앞에 추가 (`[(instance id % 100) + 100]` ¦¦ `[session id % 1000]`)|
|EXTEND_FLAG|N|확장 접두사 추가에 따른 변수와 테이블 열의 크기 조정 여부|
|SHARDED_FLAG|N|
|SESSION_FLAG|N|세션 시퀀스 여부 (세션이 사라지면 액세스한 세션 시퀀스의 상태도 없어지는 특징을 가짐)|
|KEEP_VALUE|N|[애플리케이션 연속성 (Application Continuity)](https://docs.oracle.com/en/database/oracle/oracle-database/21/racad/ensuring-application-continuity.html#GUID-E4A114A2-EA77-4037-A62A-BDFCF1E6D072) 에 대해 원래 값 유지 여부 (사용자가 해당 Owner일 경우에만 발생)|
|DUPLICATED|N|
|SHARDED|N|

<br>

### 수정
```sql
ALTER SEQUENCE owner_name.seq_name option_name value;

--INCREMENT BY
ALTER SEQUENCE owner_name.seq_name INCREMENT BY 1000;

--MAXVALUE
ALTER SEQUENCE owner_name.seq_name MAXVALUE 9999999;

--CACHE
ALTER SEQUENCE owner_name.seq_name CASHE 1000;
ALTER SEQUENCE owner_name.seq_name NOCASHE;

--CYCLE
ALTER SEQUENCE owner_name.seq_name CYCLE;
ALTER SEQUENCE owner_name.seq_name NOCYCLE;
```

<br>

### 삭제
```sql
DROP SEQUENCE owner_name.seq_name;
```

<br>
