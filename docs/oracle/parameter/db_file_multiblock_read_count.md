db_file_multiblock_read_count
===
>한 번의 I/O로 운반할 수 있는 블록의 개수를 설정 (Multi Block I/O)

### 확인
```sql
SHOW PARAMETER db_file_multiblock_read_count;
/*
NAME                          TYPE    VALUE 
----------------------------- ------- ----- 
db_file_multiblock_read_count integer 128
*/
```

<br>

### 정보
* `FULL SCAN` 작업 시 영향이 있음
* [db_block_size](./db_block.md#db_block_size) * `db_file_multiblock_read_count` <= 최대 I/O 크기

<br>
