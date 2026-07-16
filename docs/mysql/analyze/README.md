Analyze
===
>[https://dev.mysql.com/doc/refman/8.0/en/analyze-table.html](https://dev.mysql.com/doc/refman/8.0/en/analyze-table.html)

### 기본 형식
```sql
ANALYZE TABLE schema_name.table_name;
```

<br>

### 특징
1. 키 분포 분석을 수행하고 명명된 테이블에 대한 분포를 저장
1. 테이블에 대한 SELECT 및 INSERT 권한이 필요
1. View에서는 작동하지 않음
1. 파티션 별로도 분석 가능
1. 실행 중에는 읽기 잠금으로 테이블 락 발생
1. 기본적으로 바이너리 로그에 작성하여 복제본에 복제하기 때문에 로그 작성을 하지 않게 하려면 `NO_WRITE_TO_BINLOG` 또는 `LOCAL` 을 지정해야 함

<br>

### 키 분포 분석
1. 마지막 키 분산 분석 이후 테이블이 변경되지 않은 경우 테이블은 다시 분석되지 않음
1. 저장된 키 배포를 사용하여 상수 이외의 컬럼이나 다른 항목의 조인에서 테이블 조인 순서를 결정함
1. 쿼리 내의 특정 테이블에 사용할 인덱스를 결정할 때 키 배포를 사용할 수 있음
1. 저장된 키 분산 카디널리티를 확인 시 `SHOW INDEX FROM schema_name.table_name;`, `SELECT * FROM INFORMATION_SCHEMA.STATISTICS;` 로 확인 가능
1. InnoDB의 경우 각 인덱스 트리에서 인덱스 카디널리티 추정치를 업데이트하여 결정함. 그러나 추정치이므로 100% 정확하진 않음
1. `innodb_stats_persistent` 파라미터를 활성화하면 수집된 통계를 보다 정확하고 안정적으로 생성 가능. 다만 해당 파라미터를 활성화하면 정기적으로 통계 계산이 되지 않으므로 인덱스 컬럼 데이터를 크게 변경한 후 분석이 필요
1. `innodb_stats_persistent` 파라미터를 활성화한다면 `innodb_stats_persistent_sample_pages` 파라미터를 수정해 샘플링 페이지 수를 변경할 수 있음. 비활성화인 경우는 `innodb_stats_transient_sample_pages` 파라미터를 수정
1. 분석 실행하면 `INFORMATION_SCHEMA.INNODB_SYS_TABLESTATS` 테이블에서 테이블 통계가 지워지고 `STATS_INITIALIZED` 컬럼이 초기화되지 않음으로 설정됨. 이후 테이블에 액세스 시 통계가 다시 수집

<br>
