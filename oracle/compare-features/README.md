Compare Features
===

### 라이센스별 지원 기능 차이
```sql
--기능 활성화 여부 확인
SELECT * FROM V$OPTION ORDER BY PARAMETER;
```

|기능|엔터프라이즈 (EE)|스탠다드 (SE2)|
|-|-|-|
|ASH, AWR|○|✕|
|Bit-mapped indexes|○|✕|
|Change Data Capture|○|✕|
|Compression (Advanced)|○|✕|
|Compression (Advanced Index)|○|✕|
|Compression (Basic)|○|✕|
|Compression (Unused Block)|○|✕|
|Flashback Data Archive|○|○|
|Flashback Database|○|✕|
|Flashback Table|○|✕|
|Join Index|○|✕|
|Materialized View Rewrite|○|✕|
|Online Index Build|○|✕|
|Oracle Data Guard|○|✕|
|Oracle Active Data Guard|○|✕|
|Parallel Backup & Recovery|○|✕|
|Parallel Execution|○|✕|
|Parallel Load|○|○|
|Partition|○|✕|
|RAC|○|✕|
|RAT|○|✕|
|Spatial|○|✕|
|Streams Capture|○|✕|
|Table Clustering|○|✕|
>19c 기준

<br>

### AWS RDS ORACLE
>https://docs.aws.amazon.com/prescriptive-guidance/latest/evaluate-downgrading-oracle-edition/compare-features.html

<br>
