db_writer_processes
===
>DBWR 백그라운드 프로세스의 수

### 확인
```sql
SHOW PARAMETER db_writer_processes;
/*
NAME                       TYPE     VALUE
-------------------------- -------- -------
db_writer_processes        integer  4
*/
```

<br>

### 정보
* 기본값 : `1` 또는 `CPU 개수 / 8` (오라클 권장 : 최소 CPU 8개에 하나의 DBWR 프로세스를 할당)
* DBWR 백그라운드 프로세스가 하나 밖에 없을 때 I/O 대기 현상이 발생할 경우 설정
* 해당 프로세스 수를 많이 할당한다고 하여 변경된 데이터 블록을 디스크에 저장하는 속도가 빨라지는 것은 아님
* [dbwr_io_slaves](./dbwr_io_slaves.md) 파라미터에 값이 할당되면 작동하지 않음

<br>
