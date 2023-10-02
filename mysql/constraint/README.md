Constraint
===

### 확인
```sql
SELECT * FROM INFORMATION_SCHEMA.STATISTICS TABLE_NAME = 'table_name';
```

<br>

### 생성
* [CREATE TABLE](../table/README.md#table-생성) 할 때 정의하여 생성
* PK를 별도로 생성해야할 경우
    ```sql
    ALTER TABLE schema_name.table_name
    ADD PRIMARY KEY (column_name);
    # ADD PRIMARY KEY (column_name1, column_name2, column_name3 ...);
    ```

<br>
