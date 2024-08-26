Oracle Text
===

### 정의
SQL 작성시 `LIKE` 연산을 자주 사용하는데 검색문자열 앞에 `%` 가 있는 경우에는 인덱스를 사용하지 않거나 Index Fast Full Scan을 사용함
```sql
SELECT * FROM table_name WHERE column_name LIKE '%KEYWORD'; --검색 시간이 오래 걸림
```

Oracle Text는 내부적으로 키워드 딕셔너리를 구성하여 빠르게 읽어올 수 있는 방법임 (Intermedia Text, Domain Index, Text Index 등 여러가지로 불리지만 같은 단어임)

11g 부터는 Oracle Text가 기본적으로 설치되어 있으며 `CTXSYS` 유저가 관련 기능을 수행하는 유저임

<br>

### 기능 확인
```sql
SELECT * FROM DBA_REGISTRY;
```

<br>

### 생성
```sql
--1. 필요한 정책, 권한
GRANT RESOURCE, CONNECT, CTXAPP TO owner_name;
GRANT EXECUTE ON CTXSYS.CTX_CLS TO owner_name;
GRANT EXECUTE ON CTXSYS.CTX_DDL TO owner_name;
GRANT EXECUTE ON CTXSYS.CTX_DOC TO owner_name;
GRANT EXECUTE ON CTXSYS.CTX_OUTPUT TO owner_name;
GRANT EXECUTE ON CTXSYS.CTX_QUERY TO owner_name;
GRANT EXECUTE ON CTXSYS.CTX_REPORT TO owner_name;
GRANT EXECUTE ON CTXSYS.CTX_THES TO owner_name;
GRANT EXECUTE ON CTXSYS.CTX_ULEXER TO owner_name;

--2. (옵션) 데이터 스토어 지정
BEGIN
    CTX_DDL.CREATE_PREFERENCE('fds_name', 'FILE_DATASTORE');
    CTX_DDL.SET_ATTRIBUTE('fds_name', 'PATH', '/directory_name/fds');
END;

--3. Oracle Text 인덱스 생성
CREATE INDEX owner_name.index_name ON owner_name.table_name(column_name) INDEXTYPE IS CTXSYS.CONTEXT;
```

<br>

### 조회
```sql
SELECT COUNT(*) FROM owner_name.table_name WHERE CONTAINS(column_name, '%KETWORD') > 0;
```

<br>

### 갱신
```sql
--기본값으로 자동으로 갱신이 되지 않으므로 수동으로 갱신
EXEC CTX_DDL.SYNC_INDEX('owner_name.index_name');

--데이터 변경이 있을 때마다 바로 인덱스에 반영
CREATE INDEX owner_name.index_name ON owner_name.table_name(column_name) INDEXTYPE IS CTXSYS.CONTEXT
PARAMETERS (
    'SYNC(ON COMMIT)'
);

--일정 시간마다 동기화
CREATE INDEX owner_name.index_name ON owner_name.table_name(column_name) INDEXTYPE IS CTXSYS.CONTEXT
PARAMETERS (
    'SYNC(EVERY SYSDATE + 1 / 24 / 60)' --1분
);
```

<br>

### 관련 문서
* 자세한 설명은 [해당 문서](./description.md) 참고
* [ORA-29851](../../error/29851.md)

<br>
