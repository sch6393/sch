Date Format
===
>[https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_date-format](https://dev.mysql.com/doc/refman/8.0/en/date-and-time-functions.html#function_date-format)

### 자주 쓰는 표현
```sql
--년 %Y ➞ YYYY
--년 %y ➞ YY
--월 %m ➞ 00 ~ 12
--일 %d ➞ 00 ~ 31
--시 %H ➞ 00 ~ 23
--시 %h ➞ 01 ~ 12
--분 %i ➞ 00 ~ 59
--초 %S ➞ 00 ~ 59

--format
DATE_FORMAT(datetime, '%Y%m%d%H%i%S')

--character ➞ datetime
str_to_date('20220707', '%Y%m%d')
```

<br>

### 특정 범위의 날짜 리스트 출력
```sql
SELECT DATE_FORMAT(a.date_ymd, '%Y%m%d') FROM (
    SELECT CURDATE() - INTERVAL (a.a + (10 * b.a) + (100 * c.a)) DAY AS date_ymd FROM (
    SELECT 0 AS a UNION ALL SELECT 1 UNION ALL SELECT 2 UNION ALL SELECT 3 UNION ALL SELECT 4 UNION ALL SELECT 5 UNION ALL SELECT 6 UNION ALL SELECT 7 UNION ALL SELECT 8 UNION ALL SELECT 9
    ) AS a
    CROSS JOIN (
    SELECT 0 AS a UNION ALL SELECT 1 UNION ALL SELECT 2 UNION ALL SELECT 3 UNION ALL SELECT 4 UNION ALL SELECT 5 UNION ALL SELECT 6 UNION ALL SELECT 7 UNION ALL SELECT 8 UNION ALL SELECT 9
    ) AS b
    CROSS JOIN (
    SELECT 0 as a UNION ALL SELECT 1 UNION ALL SELECT 2 UNION ALL SELECT 3 UNION ALL SELECT 4 UNION ALL SELECT 5 UNION ALL SELECT 6 UNION ALL SELECT 7 UNION ALL SELECT 8 UNION ALL SELECT 9
    ) AS c
) AS a
WHERE 1 = 1 AND a.date_ymd BETWEEN '20220101' AND '20220901'
ORDER BY a.date_ymd ASC;
```

<br>
