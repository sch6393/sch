SQL Loader
===

### ctl 파일 작성
```sql
load data
infile '/AAA/BBB/CCC.csv'
append                           --insert, append, replace, truncate
into table owner_name.table_name
fields terminated by ','         --구분자
optionally enclosed by '"'       --데이터가 감싸져 있는 형태
trailing nullcols                --Null 처리
(
column_name1,
column_name2 "TO_DATE(:column_name2, 'YYYY/MM/DD HH24:MI:SS')", --Date 처리
column_name3
...
)
```

<br>

### SQL Loader
```sql
sqlldr USERID=user_name/password CONTROL='/AAA/BBB/CCC.ctl' LOG='/AAA/BBB/CCC.log' SKIP=1
```
>SKIP : 해당 라인 무시 옵션 (csv 파일의 첫 라인에 컬럼명이 있을 경우 사용)

<br>

### 기타 옵션
```sql
DIRECT = TRUE,         --다이렉트 모드
MULTITHREADING = TRUE, --멀티스레딩 처리 (다이렉트 모드에서만 사용 가능)
PARALLEL = TRUE        --병렬처리
```
|||
|-|-|
|Conventional|SGA 메모리 사용 (대기가 발생할 가능성이 있음, 다이렉트보다 느림)|
|Direct|PGA 메모리를 사용|
>다이렉트 모드를 사용하면 Path used 값이 Conventional이 아닌 Direct로 됨.

<br>

### 주의
* 다이렉트 모드의 인덱스 갱신 순서는 아래와 같으므로 PK가 `UNUSABLE` 상태가 됨
  1. 데이터를 로드하면서 인덱스 키에 해당하는 데이터는 Sort 세그먼트에 저장됨
  1. 데이터 커밋이 끝나고 기존 인덱스와 Sort 세그먼트를 Merge함
>2의 작업에서 데이터 커밋이 끝난 후 제약조건을 체크하기 때문

<br>
