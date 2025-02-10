Parameter
===
>https://www.postgresql.org/docs/current/config-setting.html

>https://pgtune.leopard.in.ua/

### 파라미터 확인
```sql
SHOW parameter_name;

SELECT * FROM PG_SETTINGS;
```

<br>

### 파라미터 수정
```sql
ALTER SYSTEM SET parameter_name=value;
ALTER SYSTEM RESET parameter_name;

--전체 초기화
ALTER SYSTEM RESET ALL;
```

<br>

### 파라미터 적용
```sql
--Query
SELECT pg_reload_conf();

--Shell
pg_ctl reload  --signup
pg_ctl restart --postmaster
```

<br>
