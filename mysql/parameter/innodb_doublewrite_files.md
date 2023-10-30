innodb_doublewrite_files
===
>Double Write Buffer 파일 개수 설정

### https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_doublewrite_files

<br>

### 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE 'innodb_doublewrite_files';
/*
+--------------------------+-------+
| Variable_name            | Value |
+--------------------------+-------+
| innodb_doublewrite_files | 2     |
+--------------------------+-------+
1 row in set (0.01 sec)
*/
```

<br>

### 관련 내용
* [Double Write Buffer](../double-write-buffer/README.md) 문서 참조
* 기본값으로 [`innodb_buffer_pool_instances`](./innodb_buffer_pool_instances.md) * 2 의 개수로 설정되며 각 Buffer Pool Instance 마다 2개의 Double Write Buffer를 생성함 (Flush List File, LRU List File)

|비교항목|Flush List File|LRU List File|
|-|-|-|
|저장|Buffer Pool의 Flush 목록에서 Flush된 페이지|Buffer Pool의 LRU 목록에서 Flush된 페이지|
|기본 크기|[`innodb_page_size`](./innodb_page_size.md) * [`innodb_doublewrite_pages`](./innodb_doublewrite_pages.md)|[`innodb_page_size`](./innodb_page_size.md) * ([`innodb_doublewrite_pages`](./innodb_doublewrite_pages.md) + (512 / [`innodb_buffer_pool_instances`](./innodb_buffer_pool_instances.md) 수))|

<br>

### 관련 파라미터
* [innodb_doublewrite](./innodb_doublewrite.md)
* [innodb_doublewrite_batch_size](./innodb_doublewrite_batch_size.md)
* [innodb_doublewrite_dir](./innodb_doublewrite_dir.md)
* [innodb_doublewrite_pages](./innodb_doublewrite_pages.md)

<br>
