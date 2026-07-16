sort_area_size
===
>정렬에 사용할 최대 메모리 양 (바이트)

### 확인
```sql
SHOW PARAMETER sort_area_size;
/*
NAME                    TYPE    VALUE 
----------------------- ------- ----- 
sort_area_size          integer 65536 
*/
```

<br>

### 변경
```sql
ALTER SYSTEM SET sort_area_size=000000;
```

<br>
