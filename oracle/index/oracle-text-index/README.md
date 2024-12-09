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
SELECT COUNT(*) FROM owner_name.table_name WHERE CONTAINS(column_name, '%KEYWORD') > 0;
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

### 삭제
```sql
PURPOSE
--------------------------------------------------------------------------------------------------
Intermedia text 나 Oracle Text 를 사용할 때, Text index는 삭제되었는 데,  meta data가 삭제되지 않은
경우가 생긴다. 이런 경우, 수동으로 meta data 까지 모두 삭제하는 방법을 알아보자.

Explanation
--------------------------------------------------------------------------------------------------
이 방법은 잘못되는 경우 CTXSYS user를 다시 초기화 해야하는 문제가 발생할 수 있으므로 이를 감안하여
작업해야 한다.

수동으로 삭제하는 방법은 다음과 같은 절차를 이용하여 작업할 수 있다.
1. 먼저 해당 user에서 다음과 같은 command로 index를 drop 해 본다.
SQL> drop index INDEX_NAME force;
2. drop command로 drop이 정상적으로 되지 않으면 다음과 같이 실행한다.
sqlplus ctxsys/ctxsys

select idx_id from dr$index where idx_name='<TEXT_INDEX_NAME>';
=> 조회된 index id가 1092 인 경우 아래와 같이 실행한다.

delete from dr$index_value where IXV_IDX_ID = 1092;
delete from dr$index_object where IXO_IDX_ID = 1092;
delete from dr$index where idx_id = 1092;
commit;
3. Text index를 생성한 user에서 DR$<index_name>$i 이름의 TABLE을 모두 다음과 같이 drop한다.
SQL> drop table dr$<index_name>$i;
SQL> drop table dr$<index_name>$k;
SQL> drop table dr$<index_name>$n;
SQL> drop table dr$<index_name>$r;
4. 이제 해당 Text index를 다시 생성하면 된다.

Reference Documents
--------------------------------------------------------------------------------------------------
<Note:133482.1>
```

<br>

### 관련 문서
* 자세한 설명은 [해당 문서](./description.md) 참고
* [ORA-29851](../../error/29851.md)

<br>
