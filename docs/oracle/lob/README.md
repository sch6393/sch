LOB (Large Objects)
===

### 종류
|종류|설명|위치|예시|
|-|-|-|-|
|CLOB|Character LOB|내부|대용량 텍스트|
|BLOB|Binary LOB|내부|이미지, 소리, 비디오 등|
|NCLOB|National Character LOB|내부|대용량 텍스트 (국가 언어 지원 기능 사용 가능)|
|BFILE|Binary Files|외부|`.mp4`, `.jpg`, 등|

<br>

### 특징
1. LOB 컬럼을 `MOVE` 했다면 리빌드를 해주어야 함
1. LOB 컬럼은 리네임 가능하지만 LOB 인덱스는 리네임할 수 없음
1. LOB 컬럼은 파티션 키로 사용할 수 없음

<br>

### 관련 옵션
* `PCTVERSION`
  * LOB 데이터가 업데이트 시 읽기 일관성을 위해 사용하는 스토리지의 빈 공간 퍼센트율 (기본값은 `10`) 
  * `10%` 까지는 이전 데이터를 들고 있다가 해당 크기를 넘어서면 덮어쓰기로 재사용함
  * Read Only 라면 `0` 으로 설정 가능

<br>

* `CACHE`, `NOCACHE`
  * 자주 액세스가 된다면 사용할 수 있음
  * 다만, 인라인 LOB는 버퍼 캐시에서 바로 읽혀지기 때문에 해당 옵션과 상관 없음

<br>

* `CHUNK`
  * LOB 데이터를 액세스하는 단위
  * 보통 [`db_block_size`](../parameter/db_block.md#db_block_size) 의 배수로 설정됨
  * 인라인 LOB에는 영향이 없고 아웃라인 LOB에는 영향이 있음

<br>

* `LOGGING`, `NOLOGGING`
  * `redo` 로그 생성 여부
  * `CACHE` 옵션을 사용한다면 `LOGGING` 이 무조건 활성화됨
  * `UNDO` 정보는 LOB 인덱스만 생성되며 LOB 데이터는 생성되지 않음

<br>

* `ENABLE STORAGE IN ROW`, `DISABLE STORAGE IN ROW`
    |비교 항목|`ENABLE`|`DISABLE`|
    |-|-|-|
    |4KB 이하 데이터|인라인에 저장<br>(테이블)|아웃라인에 저장<br>(LOB 세그먼트)|
    |4KB 초과 데이터|아웃라인에 저장<br>(LOB 세그먼트)|아웃라인에 저장<br>(LOB 세그먼트)|
    |`ROW CHAINING`|상대적으로 발생 확률이 높음|상대적으로 발생 확률이 낮음|

    |비교 항목|인라인 (테이블)|아웃라인 (LOB 세그먼트)|
    |-|-|-|
    |REDO|○|✕|
    |UNDO|○|✕<br>(Column Locator나 LOB 인덱스가 변경되는 경우만 ○)|

  * 액세스가 많지 않다면 `DISABLE` 로 설정하는 편이 HWM가 작게 유지됨 (풀스캔을 자주한다면 더욱 유용)

<br>

### 관련 함수
|표현식|설명|
|-|-|
|`DBMS_LOB.GETLENGTH(lob_column)`|LOB 컬럼의 데이터 길이|
|`DBMS_LOB.COMPARE(lob_column1, lob_column2)`|2개의 LOB 컬럼 비교|
|`DBMS_LOB.SUBSTR(lob_column, 1000, 1)`|LOB 컬럼의 데이터를 잘라서 출력 (컬럼, 크기, 시작점)|
|`DBMS_LOB.INSTR(lob_column, 'keyword', 1, 1)`|LOB 컬럼의 데이터에서 특정 문자열 검색 (컬럼, 키워드, 시작점, 몇 번째에 위치한 키워드)|

<br>
