db_block_%
===
>[Block](../block/README.md) 에 관련된 파라미터

### 확인
```sql
SHOW PARAMETER db_block;
/*
NAME              TYPE    VALUE   
----------------- ------- ------- 
db_block_buffers  integer 0       
db_block_checking string  MEDIUM  
db_block_checksum string  TYPICAL 
db_block_size     integer 8192 
*/
```

<br>

### db_block_buffers
>SGA 메모리 안에서 캐시되는 블록 수
* [자동 메모리 관리 기술](../automatic-memory-management/README.md) 이 도입된 이후, 정확히는 Dynamic SGA가 도입된 이후 해당 파라미터와 `buffer_pool_keep`, `buffer_pool_recycle` 파라미터는 의미가 없어졌으나 새로운 3개의 파라미터가 추가됨
  1. `db_cache_size`
  1. `db_keep_cache_size`
  1. `db_recycle_cache_size`

<br> 

* 변경점
  1. KEEP과 RECYCLE에 관련된 내용은 각각의 파라미터가 대체함
  1. 이전 파라미터의 정의된 값들은 Buffer 개수이지만 (실제 메모리 크기를 계산하려면 [db_block_size](#db_block_size) 를 곱셈 계산이 필요) 변경된 파라미터는 실제 메모리 크기로 변경됨
  1. 이전 파라미터에서는 `db_block_buffers` 가 `buffer_pool_keep` 과 `buffer_pool_recycle` 의 값을 포함했지만 변경된 파라미터들은 각각 독립적으로 변경됨

<br>

### db_block_checking
>디스크 또는 I/O 시스템에 의해 발생한 손상을 감지하기 위한 파라미터

|값|설명|
|-|-|
|`OFF`|시스템 테이블 스페이스만 검증|
|`LOW`|기본 블록 헤더 검증|
|`MEDIUM`|인덱스를 제외한 전체 오브젝트 검증 + `LOW` 검증|
|`FULL`|전체 오브젝트 검증 + `MEDIUM` 검증 + `LOW` 검증|

* 블록이 갱신될 때마다 메모리 상의 정보에 대해 데이터 비트 체크섬을 실시하여 블록 파손이 발생하지 않았는지 확인
* 버퍼 캐시 상의 블록을 변경한 후 논리적 정합성을 체크
* [db_block_checksum](#db_block_checksum) 이 검증되어도 논리적 / 의미적 부정 검지가 가능
* SQL의 데이터 갱신 이외에도 변경이 이루어짐 (예를 들어 DBWR 쓰기에 따른 블록 헤더 정보 업데이트)
* 오버헤드가 최대 10%까지 증가

<br>

### db_block_checksum
>디스크 또는 I/O 시스템에 의해 발생한 손상을 감지하기 위한 파라미터

|값|설명|오버헤드 증가|
|-|-|-|
|`OFF`|검사하지 않음||
|`TYPICAL`|모든 데이터 파일 대상 (하위 버전 (10g) 에서의 `TRUE` 에 해당됨)|1 ~ 2%|
|`FULL`|데이터 갱신, 삭제가 적용되기 전 버퍼 내의 데이터에도 체크섬 검증을 하고 적용 후 재계산|4 ~ 5%|

* 블록의 헤더에 체크섬을 사용함
* 블록의 마지막 쓰기 시 체크섬을 저장하고 블록의 데이터와 체크섬을 검증

<br>

### db_block_size
>데이터베이스의 기본 블록 크기
* 데이터베이스를 재생성하지 않는 이상 변경할 수 없음

<br>
