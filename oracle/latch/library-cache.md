Library Cache
===
>Library Cache Latch를 얻는 과정에서 경합이 발생해 나타나는 대기 이벤트

### 내용
SQL을 수행하기 위해 Library Cache 메모리 영역을 탐색하고 관리하는 모든 작업을 보호함

<br>

### 원인과 진단, 해결
|원인|진단 방법|해결 방안|
|-|-|-|
|파싱이 너무 많은 경우|대기가 오래 걸릴 시점 때 Parse Time Elapsed, Parse Count (Total, Hard), Execute Count 확인 (파싱 소요 시간, 발생한 파싱 횟수, SQL 수행 횟수)|바인드 사용<br>SESSION_CACHED_CURSORS 파라미터 조정|
|[Version Count](../parsing/README.md#version-count)가 높은 경우|해당 문서 참조|해당 문서 참조|
|SGA 영역에서 Page Out이 발생하는 경우|대기가 오래 걸릴 시점 때 OS에서 스왑이 발생했는지 확인|메모리를 과도하게 사용하는 프로세스가 있는지 확인|

<br>

### 관련 내용
* [Latch](./README.md)

<br>
