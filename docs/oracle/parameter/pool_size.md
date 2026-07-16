pool_size
===
>각각의 풀 크기와 관련된 파라미터

### 확인
```sql
SHOW PARAMETER pool_size;
/*
NAME                  TYPE        VALUE 
--------------------- ----------- ----- 
java_pool_size        big integer 0     
large_pool_size       big integer 0     
memoptimize_pool_size big integer 0     
olap_page_pool_size   big integer 0     
shared_pool_size      big integer 0     
streams_pool_size     big integer 0     
*/
```

<br>

### java_pool_size
>JAVA로 작성된 프로그램 뿐만이 아니라 메모리에서 JAVA 메소드 및 클래스 정의 표현, JAVA 세션 공간의 자바 객체 마이그레이션까지 포함됨

<br>

### large_pool_size
>대량의 메모리를 할당할 때 지정되는 메모리 영역 크기
* 메모리 체크
  ```sql
  SELECT POOL, NAME, BYTES / 1024 / 1024 "MB" 
  FROM V$SGASTAT
  WHERE NAME = 'free memory' AND POOL = 'large pool';
  /*
  POOL	NAME	MB
  large pool	free memory	127.6875
  */
  ```

<br>

### memoptimize_pool_size
>[MemOptized RowStore](../memoptized-rowstore/README.md) 를 사용할 때 할당되는 풀 크기

<br>

### olap_page_pool_size
>OLAP 세션에 할당할 페이징 캐시의 최대 크기 지정

<br>

### shared_pool_size
>Data Dictionary, Stored Procedure, 각종 SQL Statement가 저장됨

* SGA 영역 중 하나이며 Dictionary Cache, Library Cache로 나뉘어짐
* Hit Ratio 측정
  ```sql
  SELECT (1 - (SUM(GETMISSES) / SUM(GETS))) * 100 "Hit Ratio" 
  FROM V$ROWCACHE;
  /*
  Hit Ratio
  99.40625316
  */
  ```
* 메모리 체크
  ```sql
  SELECT POOL, NAME, BYTES / 1024 / 1024 "MB" 
  FROM V$SGASTAT
  WHERE NAME = 'free memory' AND POOL = 'shared pool';
  /*
  POOL	NAME	MB
  shared pool	free memory	1471.90719604
  */
  ```

<br>

### streams_pool_size
>공유 리소스 중 하나이며 해당 Streams 풀을 사용하는 제품, 기능은 Oracle Golden Gate, XStream, Oracle Advanced Queuing, Oracle Data Pump 가 있음

<br>
