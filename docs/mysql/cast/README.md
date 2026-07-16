Cast
===

### 기본 형식
```sql
SELECT CAST(10 / 2 AS SIGNED);
# 5

SELECT 10 / '2', 10 / 2, 10 / CAST('2' AS UNSIGNED);
/*
5
5.0000
5.0000
*/
```

|사용 가능한 타입|
|-|
|BINARY|
|CHAR|
|DATE|
|DATETIME|
|TIME|
|DECIMAL|
|JSON|
|NCHAR|
|SIGNED (INTEGER)|
|UNSIGNED (INTEGER)|

<br>
