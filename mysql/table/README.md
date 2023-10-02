Table
===

### Table 생성
```sql
CREATE TABLE schema_name.table_name (
    column_name1 data_type constraint_type default/expression,
    column_name2 data_type constraint_type default/expression,
    ...
    column_name8 data_type constraint_type default/expression,
    column_name9 data_type constraint_type default/expression,
    PRIMARY KEY (column_name),
    UNIQUE KEY (column_name),
    KEY (column_name) # INDEX
    # PRIMARY KEY (column_name1, column_name2, ... ),
    # UNIQUE KEY (column_name1, column_name2, ... ),
    # KEY (column_name1, column_name2, ... ) # INDEX
) ENGINE=engine_name DEFAULT CHARSET=character_set_name COLLATE=character_collate_name
```

<br>

### Column 변경
```sql
--추가
ALTER TABLE schema_name.table_name ADD COLUMN column_name data_type constraint_type default/expression;

--삭제
ALTER TABLE schema_name.table_name DROP COLUMN column_name;
```

<br>

### 이름 변경
```sql
RENAME TABLE schema_name.table_name TO schema_name.table_name;
```

<br>

###

