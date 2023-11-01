Semi-sync Replication
===

### 개요
Async 방식의 Replication은 Slave의 복제 여부를 확인하지 않고 Master 에서 진행 중인 트랜잭션을 종료 및 반환함

따라서 Slave에서 복제가 여러가지 이유 (네트워크나 시스템 부하 등) 로 시점에 따른 데이터 차이가 발생되는데 이를 줄이고 데이터의 일관성을 유지하기 위함

<br>

### 설명
Master로부터 Slave에 전달된 변경 내역이 Relay log에 기록되었다는 메세지를 받고 난 후 클라이언트에 결과를 반환하는 방식

Async 방식에 비하면 성능이 조금 저하되긴 하지만 Sync 방식 (Relay log에 기록된 내용이 데이터 파일까지 적용) 방식보다는 성능 저하가 적고 일단 Slave 측에서는 변경 내역은 받았기 때문에 데이터 일관성, 안정성이 Async 방식보다 더 보장됨

<br>

### 방식
* After Commit
  ```
          ----------------Master---------------      --------Slave-------
          |User Thread             Dump Thread|      |I/O Thread        |
  Commit ----->||                      ||     |      |   ||             |
          |    ||-> Wrtie Binlog ----->||     |      |   ||             |
          |    ||-> Push ACK Request ->||     |      |   ||             |
          |    ||-> Sync Binlog        ||     |      |   ||             |
          |    ||-> Engine Commit      ||-> Send Event ->||             |
          |    ||-> Wait...            ||     |      |   ||-> Relay Log |
          |    ||                      ||<-------- ACK <-||             |
          |    ||<-- Binlog Position <-||     |      |   ||             |
  OK <---------||                      ||     |      --------------------
          |    ||                      ||     |
          -------------------------------------
  ```
  1. Commit
  1. Binlog Flush, Commit
  1. Engine Commit
  1. Binlog Dump, Send Event with ACK 요청
  1. Relay log 기록
  1. ACK 수신
  1. User Commit

<br>

* After Sync 
  ```
          ----------------Master---------------      --------Slave-------
          |User Thread             Dump Thread|      |I/O Thread        |
  Commit ----->||                      ||     |      |   ||             |
          |    ||-> Wrtie Binlog ----->||     |      |   ||             |
          |    ||-> Push ACK Request ->||     |      |   ||             |
          |    ||-> Sync Binlog        ||-> Send Event ->||             |
          |    ||-> Wait...            ||     |      |   ||-> Relay Log |
          |    ||                      ||<-------- ACK <-||             |
          |    ||<-- Binlog Position <-||     |      |   ||             |
          |    ||-> Engine Commit      ||     |      --------------------
  OK <---------||                      ||     |
          |    ||                      ||     |
          -------------------------------------
  ```
  1. Commit
  1. Binlog Flush, Commit
  1. Binlog Dump, Send Event with ACK 요청
  1. Relay log 기록
  1. ACK 수신
  1. Engine Commit
  1. User Commit

<br>

* 두 방식의 차이점
  * After Commit은 Binlog Dump를 Engine Commit 이후 전송
  * After Sync는 Binlog Dump를 Binlog Flush, Commit 이후 전송함 (Engine Commit 전)

<br>
