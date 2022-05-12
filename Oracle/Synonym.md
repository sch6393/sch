Synonym
===

### 생성 권한
```sql
--owner_name에 대해 Public Synonym 생성 권한 부여
GRANT CREATE PUBLIC SYNONYM TO owner_name;
```

<br>

### 생성
```sql
CREATE SYNONYM synonym_name FOR object_name;
CREATE PUBLIC SYNONYM synonym_name FOR object_name;
```

<br>

### DB Link와 연결 시
```sql
CREATE PUBLIC SYNONYM synonym_name FOR object_name@database_link_name;
```

<br>

### 삭제
```sql
DROP SYNONYM synonym_name;
DROP PUBLIC SYNONYM synonym_name;
```
