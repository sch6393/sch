PCT
===

### PTCFREE
* Block의 데이터 사용에 대응하기 위해 확보한 Block의 % 값
  >기본 값 : 10 (%)

  |% 값|설명|
  |-|-|
  |작을 경우|데이터 사용에 대해 적은 공간을 확보<br>많은 데이터가 하나의 Block에 들어감|
  |클 경우|데이터 사용에 대해 많은 공간을 확보<br>데이터를 넣기 위해 상대적으로 많은 Block을 필요로 함|

<br>

### PCTUSED
* Block에서 데이터를 사용할 수 있는 최소 % 값
  >기본 값 : 40 (%)

  |% 값|설명|
  |-|-|
  |작을 경우|사용되지 않는 공간이 증가함<br>상대적으로 처리 속도가 빠름|
  |클 경우|사용되지 않는 공간이 감소함<br>상대적으로 처리 속도가 느림|

<br>

### 참고
1. 업데이트, 삭제가 자주 발생하며 데이터 크기가 증가가 예상된다면 PCTFREE의 비율을 높인다.
1. 테이블 용량이 크고 읽기 작업이 대부분이라면 PCTUSED의 비율을 높인다.

<br>

### 예시
```sql
--Table
CREATE TABLE owner_name.table_name (
  column_info ...
)
PCTFREE 20
PCTUSED 40;

--Index (인덱스는 PTCUSED를 사용할 수 없음)
CREATE INDEX owner_name.index_name ON table_name(column_name ...)
PCTFREE 5
TABLESPACE tablespace_name;
```

<br>
