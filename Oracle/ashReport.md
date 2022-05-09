ash Report
===

### 텍스트 파일 레포트 추출
```sql
cd /aaa/bbb/.../report
sqlplus / as sysdba @?/rdbms/admin/ashrpt
```

<br>

### 내용
|||
|-|-|
|SGA Size|System Global Area. 하나의 인스턴스에 대한 데이터, 제어정보가 저장되어 있으며 모든 서버, 프로세스, 백그라운드 프로세스로 공유되는 메모리 구조. 캐시, 데이터, 블록 및 공유 SQL 영역|
|Buffer Cache|디스크의 물리적인 I/O를 줄이기위한 캐시를 사용하는 메모리 양|
|Shared Pool|SGA의 구성요소 중 하나. Libarary Cache, Dictionary Cache, Sequence Cache 등 여러가지 캐시가 포함되어 있음.|
|ASH Buffer Size|오라클 활성 세션 기록에 필요한 내부 데이터 버퍼 크기.|
|latch|SGA 메모리를 보호하기 위한 로우 레벨의 내부 락이며 동시에 자원에 액세스 하면서 데이터 부정합 방지, 순차적으로 접근할 수 있도록 함.|
|Mutex X|Mutual Exclusion. Critical Section을 가진 스레드의 동작시간이 서로 겹치지 않도록 개별적으로 실행하게 함. 어떠한 스레드가 Critical Section을 실행하고 있다면 다른 스레드는 그 Section에 접근할 수 없고 차례가 올 때까지 대기. Library cache mutex는 Library cache latch를 대신하기 때문에 기본적으로 발생하는 이유는 Library cache latch와 동일.|
|||

<br>
