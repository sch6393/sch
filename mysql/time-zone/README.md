Time Zone
===

### 확인
```sql
SELECT @@global.time_zone, @@session.time_zone;
```

<br>

### 값 변경
```sql
SET GLOBAL time_zone = 'Asia/Tokyo';
SET time_zone = 'Asia/Tokyo';
```

<br>

### 재시작시에도 값 유지
```sql
[mysqld]
default-time-zone='Asia/Tokyo'
```

<br>
