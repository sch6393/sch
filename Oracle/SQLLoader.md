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

### 기타 옵션 옵션
```sql
DIRECT = TRUE,         --다이렉트 모드
MULTITHREADING = TRUE, --멀티스레딩 처리 (다이렉트 모드에서만 사용 가능)
PARALLEL = TRUE        --병렬처리
```
>다이렉트 모드를 사용하면 Path used 값이 Conventional이 아닌 Direct로 됨.
<br>Conventional : SGA 메모리 사용 (대기가 발생할 가능성이 있음, 다이렉트보다 느림)
<br>Direct : PGA 메모리를 사용

<br>
