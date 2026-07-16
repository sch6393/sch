Parsing
===

### 내용
* 같은 결과를 얻는 SQL이라 하더라도 SQL 표현이 다르면 새로 파싱하게 됨
* 표현식이 같은 SQL이라 하더라도 WHERE의 조건 값만이 다른 경우도 새로 파싱하게 됨. 이때 값이 바인드된 SQL은 무조건 파싱이 이루어짐
* SQL이 수행되면 Library Cache 에서 Latch를 얻고 SQL 실행 정보 (Library Cache Object) 를 검색함. 즉 Library Cache 에 SQL 정보가 있다면 LCO를 만들지 않고 바로 실행할 수 있으나 SQL 정보가 없다면 LCO를 만들게 되고 Shared Pool Latch를 얻어 저장할 공간을 확보함

<br>

### 하드 파싱
Library Cache에 SQL 정보가 없는 경우 LCO를 생성하고 해당 LCO를 저장, 실행하는 과정을 말함

<br>

### 소프트 파싱
Library Cache에 SQL 정보가 있는 경우 이미 만들어진 LCO를 통해 바로 실행하는 과정을 말함

<br>

### Child LCO
* LCO를 생성할 때도 실행되는 조건에 따라 다르게 생성되고 관리됨
* 오라클의 경우 프로시저나 테이블 같은 객체라면 스키마 이름을 같이 저장하기 때문에 유일성이 보장되지만 SQL의 경우는 SQL 텍스트 자체가 이름으로 사용되므로 실행 주체에 따라 달라지기 때문에 유일성이 보장되지 않음
    ```sql
    --예를 들어 A, B 스키마에 각각 C라는 테이블이 있는 경우
    SELECT * FROM C;

    --위의 쿼리는 실행 주체에 따라 A 스키마의 C 테이블인지 B 스키마의 C 테이블인지 바뀌게 됨
    --이런 경우 Parent LCO 1개, A가 수행한 Child LCO 1개, B가 수행한 Child LCO 1개 해서 총 3개의 LCO가 생성됨
    ```
* 관련 정보는 `V$SQL_SHARED_CURSOR` 에서 확인 가능

<br>

### Version Count
* 몇 개의 Child LCO를 가지고 있는지를 표시함 (예를 들어 Child LCO가 3개라면 Version Count는 3이 됨)
* 관련 정보는 `V$SQLAREA` 에서 확인 가능
* Child LCO가 높다는 것은 그만큼 Library Cache를 탐색하는 시간이 길어진다는 것이고 이는 Library Cache Latch 경합이 많아질 수 있음
* Version Count가 높은 쿼리를 찾아 확인해 볼 필요가 있음

<br>

### 관련 내용
* [Latch](../latch/README.md)

<br>
