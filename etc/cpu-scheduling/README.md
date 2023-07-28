CPU Scheduling
===

### 목적
* 프로세서가 만들어지고 실행될 때 필요한 시스템 자원을 효율적으로 할당하기 위함
* 처리량 극대화, 응답시간 최소화, 자원 사용의 균형, 응답 대기 배제, 반환 시간 예측 등

<br>

### 기준
1. CPU Utilization (CPU 이용률)
1. Throughput (처리율 : 단위 시간당 완료된 프로세스의 수)
1. Response Time (응답 시간 : 작업 요청 후 응답까지 걸리는 시간)
1. Waiting Time (대기 시간 : 대기 큐에서의 대기 시간)
1. Turnaround Time (반환 시간 : 작업이 대기 큐에 들어가서 완료까지 걸리는 시간)

<br>

### 종류
||Preemptive (선점)|Non Preempticve (비선점)|
|-|-|-|
|예시|프로세스 1번과 2번이 동시에 CPU 점유 가능|프로세스 1번이 CPU를 점유하고 있다면 프로세스 2번은 대기|
|장점|빠른 응답|Response Time 예상 가능|
|단점|오버헤드 발생|실행 시간이 짧은 작업도 긴 대기 시간이 발생할 수 있음|

<br>

### Preemptive 알고리즘
|종류|설명|
|-|-|
|Round Robin|순환할당방식<br>FCFS에 의해 프로세스가 CPU를 할당 받으면 Time Slice에 의해 같은 시간의 CPU를 할당<br>시분할 방식에 효과적이며 Time Slice의 크기가 중요<br>Time Slice의 크기가 커질 수록 FCFS 방식처럼 바뀜|
|SRT (Short Remaining Time)|대기 큐의 가장 짧은 실행 시간을 가진 프로세스를 먼저 선택<br>실행 중 선점도 가능|
|Multi Level Queue|작업을 그룹화하여 여러 개의 큐를 관리 및 할당<br>각 프로세스의 종류에는 `System`, `Interactive`, `Interactive Editing`, `Batch` 등이 있음<br>각각의 큐에 우선순위를 지정할 수 있고 CPU 시간은 지정된 우선순위에 의해 차등배분|
|Multi Level Feedback Queue|Multi Level Queue와 같은 방식에서 대기 시간에 따라 프로세스를 다른 큐로 옮기는 방식이 추가됨 (프로세스의 우선순위를 낮출 수도, 높일 수도 있음)|

<br>

### Non Preemptive 알고리즘
|종류|설명|
|-|-|
|FCFS (First Come First Service)|먼저 도착한 순서대로 처리<br>실행 시간이 긴 프로세스에 의해 짧은 프로세스들이 실행되지 못하고 계속 밀리는 현상이 발생 (Convoy)|
|SJF (Shortest Job First)|실행 시간이 가장 짧은 프로세스 먼저 처리<br>대기 시간의 관점에서 보면 좋음<br>FCFS와는 반대로 실행 시간이 긴 프로세스가 짧은 프로세스에게 계속 작업 순위가 밀리며 실행이 되지 못하는 현상이 발생 (Starvation)|
|Priority|각 프로세스에 우선순위를 지정하여 우선순위대로 처리<br>우선순위가 낮은 프로세스는 계속 기다리는 현상이 발생 (Starvation)<br>대기 큐에서 일정시간 대기를 했다면 우선순위를 높여주어 실행되도록 할 수 있음 (aging)|
|Deadline|작업을 명시된 시간이나 기한 내 완료하는 방식|
|HRN (Highest Response Next)|`Response Ratio = (대기 시간 + 처리 시간) / 처리 시간`<br>긴 작업과 짧은 작업의 공정성을 고려하여 Response Ratio가 높은 것을 먼저 처리<br>SJF의 단점을 보완한 방식으로 짧은 작업이나 대기 시간이 긴 작업일 수록 우선순위가 높아짐|

<br>
