Dimension
===

### 정의
레벨 키와 레벨 키 간의 관계 (Hierarchy) 를 정의함. 각 키 간은 부모-자식의 관계를 가지는데 관계는 `1 : N`

<br>

### 객체 생성
```sql
CREATE MATERIALIZED VIEW ... ;

CREATE DIMENSION owner_name.dimension_name
    --레벨키 정의
    LEVEL key_name1 IS owner_name.table_name.column_name1
    LEVEL key_name2 IS owner_name.table_name.column_name2
    LEVEL key_name3 IS owner_name.table_name.column_name3
    ...
    LEVEL key_name IS owner_name.table_name.column_name

    --레벨키 간의 관계 정의
    HIERARCHY hierarchy_name (
        key_name1 CHILD OF
        key_name2 CHILD OF
        key_name3 CHILD OF
        ...
        key_name
    )
;
```

<br>

### 생성에 필요한 두 가지
1. Fact Table : Dimension의 대상인 실제 데이터를 가진 테이블
1. Dimension Object : 데이터 간의 레벨과 구조에 대한 정보를 저장하고 있는 오브젝트

<br>

### [Query Rewite](../query-rewrite/README.md) 시의 주의
원본 테이블을 검색하는 쿼리로 만들지 말고 Dimension을 참조할 수 있도록 쿼리를 만들어야 함 (Materialized View를 사용하므로 효율성이 높아짐)

<br>
