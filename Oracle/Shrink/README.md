Shrink
===

### 실행
```sql
ALTER TABLE owner_name.table_name SHRINK SPACE;

--인덱스도 포함
ALTER TABLE owner_name.table_name SHRINK SPACE CASCADE;
```

<br>

### 작업 순서
```sql
--1. 해당 테이블, 인덱스 NOLOGGING 상태로 변경
ALTER TABLE owner_name.table_name NOLOGGING;
ALTER TABLE owner_name.index_name NOLOGGING;

--2. ROW MOVEMENT 활성화
ALTER TABLE owner_name.table_name ENABLE ROW MOVEMENT;

--3. Shrink 실행
ALTER TABLE owner_name.table_name SHRINK SPACE CASCADE;

--4. ROW MOVEMENT 비활성화
ALTER TABLE owner_name.table_name DISABLE ROW MOVEMENT;

--5. 해당 테이블, 인덱스 LOGGING 상태로 변경
ALTER TABLE owner_name.table_name LOGGING;
ALTER TABLE owner_name.index_name LOGGING;



--해당 유저 전체로 진행할 경우
SELECT 'ALTER TABLE '||owner||'.'||table_name||' NOLOGGING;' FROM all_tables  WHERE OWNER IN ('owner_name1', 'owner_name2', ... ) UNION ALL
SELECT 'ALTER INDEX '||owner||'.'||index_name||' NOLOGGING;' FROM all_indexes WHERE OWNER IN ('owner_name1', 'owner_name2', ... );

SELECT 'ALTER TABLE '||owner||'.'||table_name||' ENABLE ROW MOVEMENT;' FROM all_tables  WHERE OWNER IN ('owner_name1', 'owner_name2', ... );

SELECT 'ALTER TABLE '||owner||'.'||table_name||' SHRINK SPACE CASCADE;' FROM all_tables  WHERE OWNER IN ('owner_name1', 'owner_name2', ... );

SELECT 'ALTER TABLE '||owner||'.'||table_name||' DISABLE ROW MOVEMENT;' FROM all_tables  WHERE OWNER IN ('owner_name1', 'owner_name2', ... );

SELECT 'ALTER TABLE '||owner||'.'||table_name||' LOGGING;' FROM all_tables  WHERE OWNER IN ('owner_name1', 'owner_name2', ... ) UNION ALL
SELECT 'ALTER INDEX '||owner||'.'||index_name||' LOGGING;' FROM all_indexes WHERE OWNER IN ('owner_name1', 'owner_name2', ... );
```

<br>

### 참고
* [ORA-10631: SHRINK clause should not be specified for this object](../Error/10631.md) 에러가 발생할 경우
* [ORA-10636: ROW MOVEMENT is not enabled](../Error/10636.md) 에러가 발생할 경우

<br>
