DateFormat
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
