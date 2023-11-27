Latch
===

### 설명
오라클이 사용하는 제어 구조 중 하나. 자원에 대한 접근을 직렬화 (Serialize) 하는 것이 목적. SGA의 특정 메모리 구조체에 대한 액세스 (Libaray Cache Latch, Cache Buffers Chains Latch) 혹은 메모리 할당 (Shared pool Latch) 에 사용되거나 Redo 로그 쓰기 작업 (Redo Writing Latch) 에도 사용됨

<br>

### 특징
1. [Enqueue](#참고-자료)와는 다르게 큐로 관리되지 않으므로 먼저 요청한 프로세스가 먼저 Latch를 얻는다는 보장이 없음 (대부분 Exclusive 모드로 얻음)
1. 그러므로 [Enqueue](#참고-자료) 보다 하위 레벨에서 Locking 자체의 부하를 최소화하며 작동함

<br>

### Latch 종류 확인
```sql
--19c 기준으로 약 1100개가 있음
SELECT * FROM V$LATCHNAME;
```

<br>

### Latch 정보 확인
```sql
--각 Latch의 얻은 횟수, Sleep에 들어간 횟수 확인
--얻은 횟수 (GETS) 와 Sleep (SLEEPS) 의 비율, Sleep이 몇 번의 사이클이 돌았는지 여부 (사이클이 많을 수록 심각성 증가)
SELECT * FROM V$LATCH;
/*
ADDR	LATCH#	LEVEL#	NAME	HASH	GETS	MISSES	SLEEPS	IMMEDIATE_GETS	IMMEDIATE_MISSES	WAITERS_WOKEN	WAITS_HOLDING_LATCH	SPIN_GETS	SLEEP1	SLEEP2	SLEEP3	SLEEP4	SLEEP5	SLEEP6	SLEEP7	SLEEP8	SLEEP9	SLEEP10	SLEEP11	WAIT_TIME	CON_ID
...
(VARBYTES)	23	7	process allocation	2600548697	5453	17	1	0	0	0	0	16	0	0	0	0	0	0	0	0	0	0	0	8067	0
...
*/
```

<br>

### Latch 얻는 방식
* Willing to Wait 모드
    * Latch를 얻는데 실패한다면 얻을 수 있을 때까지 재시도를 하는 방식
    * 일반적으로는 Willing to Wait 모드로 Latch를 얻는 경우가 많음 (Redo Copy Latch는 예외)
    * 일반적인 재시도 흐름
      1. 정해진 횟수만큼 재시도
      1. Sleep에 들어간 뒤 타임아웃이 되어 재시도 (Sleep에 들어가면 Latch Free 대기 이벤트가 발생)

<br>

* No Wait 모드
    * Latch를 얻는데 실패한다면 해당 Latch에 대해서 얻는 것을 포기하는 방식
    * 그러모르 Latch 관련 대기가 발생하지 않음
    * 일반적으로 사용되는 경우
      1. 동일한 Latch가 여러 개 존재할 때 그 중 하나만 얻으면 문제가 없는 경우 (전부 실패한다면 Willing to Wait 모드로 전환)
      1. Dead Lock을 피해야 하는 경우
          1. 모든 Latch에는 `LEVEL`이 있어서 해당 `LEVEL`에 따라 Latch를 얻어야 함
          1. 이 순서가 깨지는 경우 No Wait 모드로 시도를 함
          1. 만약 Latch를 얻는데 성공했다면 그대로 진행
          1. 만약 Latch를 얻는데 실패했다면 No Wait 모드이기 때문에 Dead Lock 상태에 들어가지 않음

<br>

### Parent Latch
```sql
--Parent Latch
SELECT * FROM V$LATCH_PARENT;
```
하나의 Latch로만 운영되는 Latch를 말함 (예를 들어 Shared Pool Latch의 경우 Shared Pool 안에서 메모리 할당을 위해 얻어야하는 Latch로 시스템에 하나만 있음)

<br>

### Child Latch
```sql
--Child Latch
SELECT * FROM V$LATCH_CHILDREN;
```
동일한 기능을 가진 Child Latch들이 Set로 운영되는 Latch를 말함 (예를 들어 Cache Buffers Chains Latch의 경우 같은 이름의 Latch들이 서로 나누어서 담당함)

<br>

### Latch 항목
* [Library Cache](./library-cache.md)
* [Cache Buffers Chains](./cache-buffers-chains.md)

<br>

### 참고 자료
* [Enqueue](../enqueue/README.md)

<br>
