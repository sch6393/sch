Enqueue
===

### 설명
오라클이 사용하는 제어 구조 중 하나. 자원에 대한 접근을 직렬화 (Serialize) 하는 것이 목적. 예를 들면 TM, TX Enqueue가 있음 (테이블 데이터를 Update할 때 사용됨)

<br>

### 특징
1. [Latch](#참고-자료)와는 다르게 큐로 관리되므로 먼저 요청한 프로세스가 먼저 Lock을 얻는 방식 (Exclusive 모드 이외에도 다양한 수준의 공유를 허용함)
1. 그러므로 [Latch](#참고-자료) 보다 상위 레벨에서 자원에 대한 운영, 관리를 목적으로 두고 작동함

<br>

### Enqueue 정보 구조
* Enqueue Resource 배열과 Enqueue Lock 배열로 구성
* 정보 처리 흐름
  1. 자원에 대한 Lock이 요청되면 해당 대상을 하나의 리소스로 할당
  1. 할당된 리소스에 관련된 Lock 정보를 링크시키는 방식으로 운영
* 리소스와 Lock은 `1:M` 관계이며 하나의 리소스에 여러 개의 Lock이 연결되어 있음 (Owner, Waiter, Converter)

<br>

### Enqueue Wait의 발생 원인과 처리 흐름, 해결 방법
* 원인
  * 다른 세션이 이미 먼저 해당 자원에 대한 Lock을 가지고 있기 때문에 원할 때, 원하는 모드로 Lock을 받을 수 없기 때문
* 처리 흐름
  1. 실패한 세션은 Owner가 작업을 완료하고 자기자신의 차례가 돌아올 때 까지 (세마포어를 포스트해줄 때 까지) Waiter 또는 Converter 큐에서 대기
  1. 대기 후 타임아웃으로 Dead Lock 상황인지 확인 후 다시 대기
* 해결 방법
  1. Enqueue에 대한 요청을 최소화하고 점유 시간을 최대한 단축시키는 것이 최우선
  1. 애플리케이션 측면에서 Lock을 장시간 가지고 있는 세션과 그 세션이 수행 중인 SQL을 찾아 조사가 필요

<br>

### Enqueue 정보 확인
```sql
--리소스
SELECT * FROM V$RESOURCE;

--락
SELECT * FROM V$LOCK;
```

### 참고 자료
* [Latch](../latch/README.md)

<br>
