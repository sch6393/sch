mysqldump, mysqlpump, mydumper, mysqlshell dump의 차이
===

### 설명
```
1. mysqldump (Sequential Load, Dump)
thread 0 |****************************************|@@@@@@|~~~~~~~~|++++++++++|=======|FINISH
[싱글 스레드로 작동함]

2. mysqlpump (Parallel Load, Dump)
thread 0 |****************************************|FINISH
thread 1 |@@@@@@|=======|
thread 2 |~~~~~~~~|
thread 3 |++++++++++|
[chunk 기능이 없어 하나의 스레드에 하나의 테이블만 작동하게 됨]

3. mydumper (Parallel chunked Load, Dump)
thread 0 |**********|++++++++++|FINISH
thread 1 |**********|@@@@@@|
thread 2 |**********|=======|
thread 3 |**********|~~~~~~~~|
[여러 chunk로 분할된 테이블은 모든 스레드에서 동시에 작동해야 함]

4. mysqlshell dump (Parallel chunked Scheduler Load, Dump)
thread 0 |**********|********|FINISH
thread 1 |@@@@@@|=======|****|
thread 2 |~~~~~~~~|**********|
thread 3 |++++++++++|********|
```

<br>

### 참고
* [mysqldump](../mysqldump/README.md)
* [mysql shell dump](../mysql-shell-dump/README.md)

<br>