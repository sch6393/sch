Tablespace
===

### Tablespace 확인  
```sql
SELECT * FROM DBA_DATA_FILES;

SELECT
  SUBSTR(A.TABLESPACE_NAME, 1, 10)        AS TBS
  ,ROUND(Capacity / 1024 / 1024, 2)       AS CapacityMB
  ,ROUND(FreeSpace / 1024 / 1024, 2)      AS FreeSpaceMB
  ,ROUND((FreeSpace / Capacity * 100), 2) AS FreeRate 
FROM (
  SELECT TABLESPACE_NAME, SUM(BYTES) AS Capacity FROM DBA_DATA_FILES GROUP BY TABLESPACE_NAME
) A 
INNER JOIN (
  SELECT TABLESPACE_NAME, SUM(BYTES) AS FreeSpace FROM DBA_FREE_SPACE GROUP BY TABLESPACE_NAME
) B ON (A.TABLESPACE_NAME = B.TABLESPACE_NAME)
ORDER BY FREESPACEMB DESC;
```

<br>

### Tablespace 추가
```sql
CREATE TABLESPACE tablespace_name DATAFILE '/AAA/BBB/CCC/tablespace_filename' SIZE size AUTOEXTEND ON NEXT size MAXSIZE UNLIMITED;

CREATE TABLESPACE tablespace_name DATAFILE '/AAA/BBB/CCC/tablespace_filename' SIZE size AUTOEXTEND OFF;
```
* 데이터 할당
  * DMT : Data Dictionary에서 공간할당을 담당. 지금은 쓰지 않는 방식
  * LMT : Extent마다 1비트를 할당해 0, 1로 공간이 있는지 없는지 표시하며 저장. 
    >공간이 비어있을 수 있음

<br>

* 데이터 구조
  * MSSM : Segment Header에서 프리리스트로 여유공간을 갖는 방식.
  * ASSM : Segment Header가 각 Extents의 Header들을 Point하고 있는 계층구조. 

<br>

* __사용 가능한 방식__
  1. LMT + ASSM (Default)
      ```sql
      EXTENT MANAGEMENT LOCAL SEGMENT SPACE MANAGEMENT AUTO;
      ```
  2. LMT + MSSM
      ```sql
      EXTENT MANAGEMENT LOCAL SEGMENT SPACE MANAGEMENT MANUAL;
      ```
  3. DMT + MSSM
      ```sql
      EXTENT MANAGEMENT DICTIONARY SEGMENT SPACE MANAGEMENT MANUAL;
      ```

<br>

* __사용 불가능한 방식__
  * DMT + ASSM (불가능)
      ```sql
      EXTENT MANAGEMENT DICTIONARY SEGMENT SPACE MANAGEMENT AUTO;
      ```

<br>

* 참조
  * DBCA로 DB를 생성
    * System Tablespace : LMT, Datafile : LMT
  * 수동으로 DB를 생성
    * System Tablespace : DMT, Datafile : LMT, DMT

<br>

### 

<br>
