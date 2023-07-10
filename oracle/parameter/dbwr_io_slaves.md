dbwr_io_slaves
===
>DBWR 슬레이브 프로세스의 수

### 확인
```sql
SHOW PARAMETER dbwr_io_slaves;
/*
NAME                        TYPE    VALUE 
--------------------------- ------- ----- 
dbwr_io_slaves              integer 0 
*/
```

<br>

### 정보
* 기본값 : `0`
* DBWR 백그라운드 프로세스를 하나 밖에 사용할 수 없거나 시스템 Asynchronous I/O를 지원하지 않을 경우 사용
* 해당 파라미터에 값이 할당되면 [db_writer_processes](./db_writer_processes.md) 는 작동하지 않음

<br>
