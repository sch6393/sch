Load Data
===

>[https://dev.mysql.com/doc/refman/8.0/en/load-data.html](https://dev.mysql.com/doc/refman/8.0/en/load-data.html)

### 기본 형식
```sql
LOAD DATA
    [LOW_PRIORITY | CONCURRENT] [LOCAL]
    INFILE 'file_name'
    [REPLACE | IGNORE]
    INTO TABLE tbl_name
    [PARTITION (partition_name [, partition_name] ...)]
    [CHARACTER SET charset_name]
    [{FIELDS | COLUMNS}
        [TERMINATED BY 'string']
        [[OPTIONALLY] ENCLOSED BY 'char']
        [ESCAPED BY 'char']
    ]
    [LINES
        [STARTING BY 'string']
        [TERMINATED BY 'string']
    ]
    [IGNORE number {LINES | ROWS}]
    [(col_name_or_user_var
        [, col_name_or_user_var] ...)]
    [SET col_name={expr | DEFAULT}
        [, col_name={expr | DEFAULT}] ...]
```

<br>

### 옵션 설명
```sql
-- csv 파일 위치 지정 (S3가 마운트 되어있다면 S3의 디렉토리도 지정 가능)
INFILE

-- 데이터를 넣을 테이블 지정
INTO TABLE

-- 필드 파싱 방법 지정
FIELDS 
TERMINATED BY '\t' -- 구분자
OPTIONALLY
ENCLOSED BY ''     -- 데이터가 감싸져 있는 형태
ESCAPED BY '\\'    -- Backslash 문자 치환

-- 라인 파싱 방법 지정
LINES
TERMINATED BY '\n' -- 구분자
STARTING BY ''     -- 시작 문자

--필드, 라인 옵션 기본값 (따로 지정하지 않을 경우)
FIELDS TERMINATED BY '\t' ENCLOSED BY '' ESCAPED BY '\\'
LINES TERMINATED BY '\n' STARTING BY ''

-- 해당 데이터 무시
IGNORE
```

<br>

### LOAD DATA를 사용하기위한 권한 설정
```sql
-- 확인
SHOW VARIABLES LIKE 'local_infile';
/*
Variable_name, Value
local_infile	ON
*/

-- DB 접속 시 해당 파라미터 값을 1로 넘김
load_infile=1

-- DB 파라미터 값 변경
SET GLOBAL local_infile = 1;
```

<br>

### Backslash 문자로 인한 오류가 발생한다면
```sql
-- ESCAPED BY 옵션을 아래와 같이 지정
ESCAPED BY ''
```
