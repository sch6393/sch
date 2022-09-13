REST API
===

### 설계 방법
1. URI는 데이터를 표현해야 함
1. 데이터 형식은 METHOD로 표현

<br>

### 예시
1. 데이터를 가져오는 URI
    ```
    GET /members/view/1    (x)
    GET /members/1         (o)
    ```

1. 데이터를 추가할 경우
    ```
    GET  /members/insert/2 (x)
    POST /members/2        (o)
    ```

<br>

### METHOD
|Name|Description|
|-|-|
|GET|데이터 조회|
|POST|데이터 생성|
|PUT|데이터 수정|
|DELETE|데이터 삭제|

<br>

### 주의점
1. `/` 는 계층 관계를 나타날 때 사용
1. URI 마지막 문자로 `/` 가 없도록 함
1. URI에 확장자를 포함하지 않도록 함 (Accept Header를 사용)
1. `_` 대신 `-` 를 사용
1. 소문자로만 구성 ([https://www.rfc-editor.org/rfc/rfc3986](https://www.rfc-editor.org/rfc/rfc3986))
1. Collection과 Document로 표현할 것

<br>

### 응답 상태 코드
|Code|Description|
|-|-|
|200|요청을 정상적으로 실행|
|201|요청을 정상적으로 실행 (POST로 데이터 생성 시)|
|301|요청 URI가 변경되었을 경우 (Location Header에 변경된 URI를 포함)|
|400|요청이 잘못됨|
|401|인증되지 않음|
|403|반환하고 싶지 않는 데이터가 있을 경우 (이 경우 400이나 404를 권장, 403의 경우 데이터가 있다는 의미이기 때문)|
|404|해당 요청에 대한 데이터를 찾을 수 없을 경우|
|405|해당 요청에 맞지 않는 METHOD를 사용한 경우|
|500|서버에 문제 발생|

<br>
