Spatial
===
https://dev.mysql.com/doc/refman/8.0/en/spatial-types.html

### 설명
공간 데이터베이스. 공간에 존재하는 점, 선, 폴리곤 등을 포함하는 객체 데이터를 저장, 검색하는데에 사용

<br>

### 데이터 타입
|TYPE|DESCRIPTION|EXAMPLE|
|-|-|-|
|POINT|좌표의 한 지점|`POINT(2 4)`|
|LINE STRING|다수의 POINT를 연결하는 선|`LINESTRING(1 1, 2 2, 3 3)`|
|POLYGON|선들이 연결되어 닫혀있는 상태|`POLYGON(1 1, 1 4, 4 1, 1 1)`|
|MULTI POINT|POINT 집합|`MULTIPOINT(2 4, 4 6)`|
|MULTI LINE STRING|LINE STRING 집합|`MULTILINESTRING((1 1, 2 2), (5 5, 4 4))`|
|MULTI POLYGON|POLYGON 집합|`MULTIPOLYGON((1 1, 1 4, 4 1, 1 1), (5 5, 7 7, 8 8, 5 5))`|
|GEOMETRY COLLECTION|모든 공간 데이터 타입의 집합|`GEOMETRYCOLLECTION(POINT(2 4), LINESTRING(1 1, 2 2, 3 3))`|

<br>

### 함수
https://dev.mysql.com/doc/refman/8.0/en/spatial-function-reference.html

<br>
