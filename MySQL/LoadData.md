Load Data
===

### 1. 기본 형식
```sql
LOAD DATA LOCAL INFILE '/aaa/data.csv'
INTO TABLE `schema`.`table`
FIELDS
  TERMINATED BY ','
  OPTIONALLY ENCLOSED BY '"' ESCAPED BY '"'
LINES
  TERMINATED BY '\n'
IGNORE 1 LINES;
```
>S3가 마운트 되어있다면 S3의 디렉토리도 지정 가능

