Oracle Text 인덱스와 `%` 의 성능 비교
===

### `LIKE '%' || :1 || '%'` + INDEX RANGE SCAN
>힌트 지정 없음

인덱스 풀스캔이 되어야 하지만 인덱스 범위 스캔으로 표시됨. 이는 인덱스 범위 스캔이 인덱스 풀스캔과 똑같은 범위를 읽기 때문 __(읽은 I/O 수치가 동일하다면 인덱스 범위 스캔이라고 표시되어도 실제 읽는 범위는 인덱스 풀스캔과 똑같다는 의미)__

<br>

### `LIKE '%' || :1 || '%'` + INDEX FULL SCAN
>`/*+ INDEX( TABLE (COLUMN) ) */`

인덱스 풀스캔으로 동작하도록 힌트 지정

<br>

### `LIKE '%' || :1 || '%'` + FTS
>`/*+ FULL(TABLE) */`

Index Scan은 Single Block I/O 이므로 Segment의 전체 Block을 읽어야 하는 상황이라면, MultiBlock I/O인 FTS, Index Fast Full Scan으로 동작하도록 하는 편이 효율적임.

FTS의 경우 Buffer가 높아지지만 실행 시간이 빨라짐. __(MultiBlock I/O가 전체를 읽을 경우 좋은 대안이 될 수 있음)__

<br>

### `LIKE '%' || :1 || '%'` + INDEX FAST FULL SCAN
>`/*+ INDEX_FFS(TABLE (COLUMN) ) */`

Index Scan은 Single Block I/O 이므로 Segment의 전체 Block을 읽어야 하는 상황이라면, MultiBlock I/O인 FTS, Index Fast Full Scan으로 동작하도록 하는 편이 효율적임.

Index Fast Full Scan의 경우 Buffer I/O가 FTS에 비해 줄어듬. __(FTS 보다 인덱스가 전체 블록이 적기 때문에 Index Fast Full Scan으로 원하는 값을 낼 수 있다면 FTS 보다 더 좋은 대안이 될 수 있음)__

<br>

### `INSTR` + INDEX FAST FULL SCAN
>`/*+ INDEX_FFS(TABLE (COLUMN) ) */`

만약 컬럼이 `NULLABLE` 일 경우 `INSTR` 에서는 Index Fast Full Scan이 동작하지 않음. 이런 경우 WHERE 절에 `column_name IS NOT NULL` 의 조건으로 Index Fast Full Scan이 동작하도록 할 수 있음

<br>

### Oracle Text 인덱스
>힌트 지정 없음

`'%' || :1 || '%'` 를 쓸 수 밖에 없는 상황에서 최적 성능은 Domain Index가 제일 좋음. 하지만 유지보수 측면에서 좋은 편이 아니기 때문에 운영측면에 대해 검토를 충분히 하고 진행해야 함 __(조회 성능이 무조건 필요한 상황이라면 Domain Index를 사용할 수 밖에 없음)__

<br>
