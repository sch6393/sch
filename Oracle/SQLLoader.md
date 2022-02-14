SQL Loader
===

### ctl 파일 작성
```
load data
infile '/AAA/BBB/CCC.csv'
append
into table table
fields terminated by ','
optionally enclosed by '"'
(column_name1, column_name2, column_name3 ...)
```

<br>

### ctl 파일 옵션
```
OPTIONS (
	--다이렉트 모드
	--멀티스레딩 처리 (다이렉트 모드에서만 사용 가능)
	--병렬처리
	DIRECT = TRUE,
	MULTITHREADING = TRUE,
	PARALLEL = TRUE
)
```

<br>

### SQL Loader
```sql
sqlldr USERID=user_name/password CONTROL='/AAA/BBB/CCC.ctl' LOG='/AAA/BBB/CCC.log' skip=1
```
>skip : 해당 라인 무시 옵션 (csv 파일의 첫 라인에 컬럼명이 있을 경우 사용)
