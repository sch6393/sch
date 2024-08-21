PFILE, SPFILE
===

### 기본
오라클은 기동 시에 파라미터 파일을 읽어서 기동하도록 되어있음
```
                             STARTUP
------------------------------------>
| SHUTDOWN |
    /|\    | NOMOUNT |
     |     |   /|\   | MOUNT |
     |     |    |            | OPEN |
     |     |    |
     |     |    |
     |  SPFILE --
     |     |   /|\
     |    \|/   |
     --- PFILE --

SPFILE이 없다면 PFILE을 찾고 PFILE도 없다면 기동 FAIL

정확히는 아래의 파일 순서로 참조
1. spfile[SID].ora
2. spfile.ora
3. init[SID].ora
4. init.ora
```

<br>

### PFILE
오라클 8에서 사용했던 형식이며 정적 파라미터 파일

위치 : `$ORACLE_HOME/dbs/initSID.ora`

<br>

### SPFILE
오라클 9부터 추가된 형식이며 동적 파라미터 파일 (바이너리이기 때문에 직접 편집 불가능)

위치 : `$ORACLE_HOME/dbs/spfileSID.ora`

<br>

### SPFILE을 PFILE로 변환, 작성한 PFILE을 SPFILE로 변환
```sql
--SPFILE을 기반으로 PFILE 생성
CREATE
    PFILE = '/directory_name/init_pfile_name.ora'
FROM
    SPFILE = '/directory_name/spfile_name.ora'
;

--PFILE을 기반으로 SPFILE 생성
CREATE
    SPFILE = '/directory_name/spfile_name.ora'
FROM
    PFILE = '/directory_name/init_pfile_name.ora'
;
```

<br>
