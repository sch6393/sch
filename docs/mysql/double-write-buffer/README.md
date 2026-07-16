Double Write Buffer
===
>https://dev.mysql.com/doc/refman/8.0/en/innodb-doublewrite-buffer.html

### 개요
* InnoDB 스토리지 엔진은 Redo Log 공간의 낭비를 막고 빠르게 쓰기 위해 페이지의 변경된 내용만 기록함 (Oracle의 경우 Redo에 Change Vector로 생성하고 있으며 블록 변경의 최소 단위)
* InnoDB의 스토리지 엔진에서 Dirty Page를 디스크 파일로 Flush 할 때 페이지 중 일부만 디스크에 기록된 상태에서 장애가 발생할 경우 그 페이지는 복구 할 수 없을 가능성이 있음
* 페이지가 일부만 기록 되는 Partial Page 또는 Torn Page는 하드웨어 오작동, 시스템의 비정상 종료 등으로 발생할 수 있음
* InnoDB 스토리지 엔진에서는 위와 같은 현상을 막기 위해 Double-Write 기법을 사용

<br>

### 설명
* Double Write Buffer (이중 쓰기 버퍼)는 InnoDB가 InnoDB 데이터 파일에 페이지를 쓰기 전, 버퍼 풀에서 Flush된 페이지를 쓰는 저장 영역임
* 페이지 쓰기 도중에 장애가 발생할 경우 InnoDB는 Crash Recovery를 하게 되고 복구 중에 필요한 페이지를 Double Write Buffer에서 찾을 수 있게 됨
* 데이터가 두 번 기록되지만 두 배의 I/O 작업을 필요로 하지 않는데 데이터를 쓸 때 Sequential I/O로 처리하기 때문

<br>

### 저장 위치
MySQL `8.0.20` 전에는 Double Write Buffer 영역이 InnoDB 시스템 테이블스페이스였으나 `8.0.20` 이후 별도의 테이블스페이스에 저장되고 저장되는 위치도 별도로 설정 가능
```sql
SHOW VARIABLES LIKE 'innodb_data_home_dir';
```

<br>

### 작동 방식
```
              -InnoDB Buffer Pool--
              |A| | | | | | | | | |
              | | | | | |D| | | | |
              | | |B| | | | | |C| |
              ---------------------
                        |
          -----------------------------
          | 1. 버퍼 영역에            | 2. 데이터 파일에 
         \|/   먼저 쓰고             \|/   하나씩 랜덤으로 씀
-Double Write Buffer-            -Data File-
|     |A|B|C|D|     |            |         |
---------------------            -----------
```
A부터 D까지의 Dirty Page를 디스크로 Flush 한다고 했을 때 InnoDB 스토리지 엔진은 실제 데이터 파일에 변경 내용을 쓰기 전, A부터 D까지의 Dirty Page를 묶어서 한 번의 디스크에 쓰기 작업으로 Double Write Buffer에 씀. 그 후 각각의 Dirty Page를 각각의 파일에 하나씩 랜덤으로 씀

만약 'A'와 'B' 페이지는 정상적으로 기록됐는데 'C' 페이지가 기록되는 도중에 장애가 발생했다고 가정했을 시 InnoDB 스토리지 엔진은 재시작 될 때 항상 Double Write Buffer 내용과 데이터 파일의 페이지들을 모두 비교해서 다른 내용을 담고 있는 페이지가 있다면 Double Write Buffer 내용을 데이터 파일의 페이지로 복사

<br>

### 관련 파라미터
* [innodb_doublewrite](../parameter/innodb_doublewrite.md)
* [innodb_doublewrite_batch_size](../parameter/innodb_doublewrite_batch_size.md)
* [innodb_doublewrite_dir](../parameter/innodb_doublewrite_dir.md)
* [innodb_doublewrite_files](../parameter/innodb_doublewrite_files.md)
* [innodb_doublewrite_pages](../parameter/innodb_doublewrite_pages.md)

<br>
