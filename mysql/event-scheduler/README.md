Event Scheduler
===

### 설정 확인
```sql
SHOW VARIABLES LIKE 'event_scheduler';
/*
Variable_name	Value
event_scheduler	OFF
*/

--ON으로 설정
SET GLOBAL event_scheduler = ON;
```
>ON으로 되어 있지 않으면 동작하지 않음

<br>

### 조회
```sql
SELECT * FROM information_schema.events;
SHOW EVENTS FROM classicmodels;
```

<br>

### 생성
```sql
CREATE EVENT event_name
ON SCHEDULE EVERY 1 WEEK             --주기
STARTS '2023-03-13 00:09:00'         --해당 시간부터 시작
ENDS   '2024-04-23 00:21:00'         --해당 시간까지 동작
ON COMPLETION PRESERVE               --동작 이후 이벤트 삭제 X
COMMENT 'comment'                    --주석
DO
...                                  --동작 내용
END
;
```
|사용 가능한 Interval|
|-|
|YEAR|
|QUARTER|
|MONTH|
|DAY|
|HOUR|
|MINUTE|
|WEEK|
|SECOND|
|YEAR_MONTH|
|DAY_HOUR|
|DAY_MINUTE|
|DAY_SECOND|
|HOUR_MINUTE|
|HOUR_SECOND|
|MINUTE_SECOND|

<br>

### 삭제
```sql
DROP EVENT event_name;
```

<br>

### 시간 기준
|키워드|`EVENTS` TABLE|`SHOW EVENTS`|
|-|-|-|
|Execute at|ETZ|ETZ|
|Starts|ETZ|ETZ|
|Ends|ETZ|ETZ|
|Last executed|ETZ|n/a|
|Created|STZ|n/a|
|Last altered|STZ|n/a|

<br>
