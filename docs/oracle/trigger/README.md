Trigger
===

### 기본 형태
```sql
CREATE OR REPLACE TRIGGER owner_name.trigger_name
AFTER INSERT OR UPDATE OR DELETE ON owner_name.table_name_source
FOR EACH ROW
BEGIN
    IF INSERTING THEN
        INSERT INTO owner_name.table_name_target VALUES (:NEW.column_name,...);
    ELSIF UPDATING THEN
        UPDATE owner_name.table_name_targetJ SET column_name=:NEW.column_name WHERE column_name=:OLD.column_name;
    ELSIF DELETING THEN
        DELETE FROM owner_name.table_name_target WHERE column_name=:OLD.column_name;
    END IF;
END;
```

<br>

### 트리거 상태 확인
```sql
SELECT OWNER, TRIGGER_NAME, STATUS FROM DBA_TRIGGERS WHERE TRIGGER_NAME = 'trigger_name';

--활성화
ALTER TRIGGER owner_name.trigger_name ENABLE;
```

<br>
