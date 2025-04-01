oracle_fdw
===

### 설치
```sql
CREATE EXTENSION oracle_fdw;
```

<br>

### 설정 순서
```sql
-- 1. FOREIGN DATA WRAPPER 생성
CREATE SERVER server_name FOREIGN DATA WRAPPER oracle_fdw OPTIONS (
    dbserver 'host:port/SID'
);

-- 2. 유저 맵핑
CREATE USER MAPPING FOR user_name SERVER server_name OPTIONS (
    user 'user_name', password 'password'
);

-- 3. FOREIGN TABLE 생성
CREATE FOREIGN TABLE foreign_table_name (column_name1 column_type1, column_name2 column_type2, column_name3 column_type3, ... )
SERVER server_name OPTIONS (table 'table_name');

-- Sample (V$VERSION)
CREATE FOREIGN TABLE foreign_table_name (banner text, banner_full text, banner_legacy text, con_id text)
SERVER server_name OPTIONS (table 'V$VERSION');
```

<br>

### 조회
```sql
SELECT * FROM foreign_table_name;
```

<br>
