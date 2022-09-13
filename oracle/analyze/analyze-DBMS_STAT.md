Analyze와 DBMS_STATS의 차이
===

### 차이점
|구분|Analyze|DBMS_STATS|
|-|-|-|
|Gathering|Serial Statistics Gathering|Parallel Gathering|
|형식|명령어|패키지|
|파티션 통계 정보|부정확할 가능성이 있음|정확|
|전체 클러스터 통계 정보|O|X|
|전체 테이블 통계 정보|O|CBO 관련 정보만 수집|
|특정 테이블 지정|X|O|
|통계 정보 저장 및 딕셔너리 반영|X|O|

<br>

### 참고
* Analyze 실행 후 DBMS_STATS을 실행하면 `ADMIN_STAT`이랑 `OPTIMIZER_STAT`이 갱신됨
* [Gathering](../gathering/README.md)

<br>
