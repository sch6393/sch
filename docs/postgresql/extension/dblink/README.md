dblink
===

### 설치
```sql
CREATE EXTENSION dblink;
```

<br>

### 사용법
```sql
--SELECT일 경우
SELECT * FROM dblink(
    'host=0.0.0.0 dbname=DBNAME user=USERNAME password=PASSWORD',
    'SELECT col1, col2 FROM table_name'
) AS t1(col1 data_type, col2 data_type);

--INSERT, UPDATE, DELETE일 경우
SELECT dblink_exec(
    'host=0.0.0.0 dbname=DBNAME user=USERNAME password=PASSWORD',
    'INSERT INTO table_name(col1, col2) VALUES (value1, value2)'
);

--프로시저일 경우
SELECT dblink_exec(
    'host=0.0.0.0 dbname=DBNAME user=USERNAME password=PASSWORD',
    $COMMAND$
    CREATE OR REPLACE PROCEDURE procedure_name(col data_type)
    LANGUAGE plpgsql
    AS $PROC$
    BEGIN
        --Procedure Content
        RAISE NOTICE 'Log: %', col);
    END;
    $PROC$;
    $COMMAND$
);
```

<br>

### 연결 유지
```sql
-- 1. 연결
SELECT dblink_connect('CONNECTION_NAME', 'host=0.0.0.0 dbname=DBNAME user=USERNAME password=PASSWORD');

-- 2. 사용
SELECT * FROM dblink('CONNECTION_NAME', 'SELECT col1, col2 FROM table_name') AS t1(col1 data_type, col2 data_type);

-- 3. 해제
SELECT dblink_disconnect('CONNECTION_NAME');

```

<br>
