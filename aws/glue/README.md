Glue
===
>[https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html](https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html)

### 기본 설정 (csv ➞ parquet)
1. S3 Bucket 및 폴더 설정
    * 디렉토리 구조
      ```
      AWS S3
      ├ bucket-glue
      │  ├ athena
      │  ├ glue-data
      │  │  └ database-test
      │  │      └ table-test
      │  │          ├ csv-test
      │  │          │  ├ dataload=YYYYMMDD
      │  │          │  │  └ YYYYMMDD.csv
      │  │          │  ├ dataload=YYYYMMDD
      │  │          │  │  └ YYYYMMDD.csv
      │  │          │  ├ dataload=YYYYMMDD
      │  │          │  │  └ YYYYMMDD.csv
      │  │          │   ...
      │  │          └ parquet-test
      │  ├ glue-log
      │  ├ glue-script
      │  └ glue-temporary
      ...
      ```

1. Glue Data Catalog 설정
    * Database
      1. Add Database
          |항목|설정 값|
          |-|-|
          |Name|`database-test`|
          |Location|`s3://bucket-glue/glue-data/database-test/`|
    * Crawlers
      1. Set crawler properties
          |상위 항목|항목|설정 값|
          |-|-|-|
          |Crawler details|Name|`crawler-test`|
      1. Choose data sources and classifiers
          |상위 항목|항목|설정 값|
          |-|-|-|
          |Data source configuration|Is your data already mapped to Glue tables?|`Noy yet`|
          |Add data source|Data source|`S3`|
          ||S3 path|`s3://bucket-glue/glue-data/database-test/table-test/csv-test/`|
          ||Subsequent crawler runs|상황에 맞게 선택|
      1. Configure security settings
          |상위 항목|항목|설정 값|
          |-|-|-|
          |IAM role|Existing IAM role|권한 선택|
      1. Set output and scheduling
          |상위 항목|항목|설정 값|
          |-|-|-|
          |Output configuration|Target database|`database-test`|
          |Crawler schedule|Frequency|상황에 맞게 선택|
      1. Run Crawler
          * Table, 파티션 정보가 업데이트됨
    * Tables (Crawler를 사용하지 않는 경우)
      1. Add Table
          |상위 항목|하위 항목|설정 값|
          |-|-|-|
          |Table details|Name|`csv-test`|
          ||Database|`database-test`|
          |Data store|Select the type of source|`S3`|
          ||Include path|`s3://bucket-glue/glue-data/database-test/table-test/csv-test/`|
          |Data format|Classification|`CSV`|
          ||Delimeter|테스트 파일에 맞는 형식으로 선택|
      1. Choose or define schema
          |상위 항목|하위 항목|설정 값|
          |-|-|-|
          |Schema|Schema|`Define or upload schema`|
          |Add Schema entry|Set as partition key?|◉|
          ||Partition index #|`1`|
          ||Name|`dataload`|
          ||Data type|`smallint`|
    * ETL Job (Parquet 변환)
      1. Create Job (S3 ➞ S3)
          |상위 항목|하위 항목|설정 값|
          |-|-|-|
          |Data Source (S3)|Name|`S3-bucket`|
          ||S3 source type|`Data Catalog table`|
          ||Database|`database-test`|
          ||Table|`table_test`|
          |Data Target (S3)|Name|`S3-bucket`|
          ||Format|`Parquet`|
          ||Compression Type|상황에 맞게 선택|
          ||S3 Target Location|`s3://bucket-glue/glue-data/database-test/table-test/parquet-test/`|
          ||Data Catalog update options|`Do not update the Data Catalog`|
      1. Job Details
          |항목|설정 값|
          |-|-|
          |Script path|`s3://bucket-glue/glue-script`|
          |Spark UI logs path|`s3://bucket-glue/glue-log`|
          |Temporary path|`s3://bucket-glue/glue-temporary`|
      1. Run ETL Job
          * Parquet 파일 생성
    * [Athena](../athena/README.md)
      1. 데이터 확인

<br>

### 에러 관련 내용
* 보안 그룹 관련 에러
  ```
  InvalidInputException: At least one security group must open all ingress ports.To limit traffic, the source security group in your inbound rule can be restricted to the same security group
  ```
  >[https://docs.aws.amazon.com/glue/latest/dg/glue-troubleshooting-errors.html#error-inbound-self-reference-rule](https://docs.aws.amazon.com/glue/latest/dg/glue-troubleshooting-errors.html#error-inbound-self-reference-rule)

* S3 엔드포인트 관련 에러
  ```
  InvalidInputException: VPC S3 endpoint validation failed for SubnetId: subnet-_________________. VPC: vpc-_________________. Reason: Could not find S3 endpoint or NAT gateway for subnetId: subnet-_________________ in Vpc vpc-_________________
  ```
  >[https://docs.aws.amazon.com/glue/latest/dg/glue-troubleshooting-errors.html#error-s3-subnet-vpc-NAT-configuration](https://docs.aws.amazon.com/glue/latest/dg/glue-troubleshooting-errors.html#error-s3-subnet-vpc-NAT-configuration)

* Glue 역할 권한 관련 에러
  ```
  LAUNCH ERROR | Error downloading from S3 for bucket: bucket-lue, key: script/job-test.py.Access Denied (Service: Amazon S3; Status Code: 403; Please refer logs for details.
  ```
  ```
  --Cloud Watch Error
  [cfe1e0b1-41f7-4bae-b031-85d3b18c7d41] ERROR : Not all read errors will be logged. com.amazonaws.services.s3.model.AmazonS3Exception: Access Denied (Service: Amazon S3; Status Code: 403; Error Code: AccessDenied; Request ID: KVHQQAERY04GCEWX; S3 Extended Request ID: uPI/W9UZKWBHzHdOEtDRNxeP+27o1QZS4LYz8yJ6rYxGY7Y6P+XMft5UMT+116oHaBdhN7QZP4w=; Proxy: null), S3 Extended Request ID: uPI/W9UZKWBHzHdOEtDRNxeP+27o1QZS4LYz8yJ6rYxGY7Y6P+XMft5UMT+116oHaBdhN7QZP4w=
  ```
  >Glue 역할에 S3 권한을 추가

* 기타 연결 실패 시 [해당 문서 참조](https://repost.aws/knowledge-center/glue-test-connection-failed)

<br>

### Google BigQuery 커넥터로 연결
* [https://repost.aws/articles/ARvGO4zmZbSkm8RpuWuVso-w/articles/ARvGO4zmZbSkm8RpuWuVso-w/push-down-queries-when-using-the-google-bigquery-connector-for-aws-glue?](https://repost.aws/articles/ARvGO4zmZbSkm8RpuWuVso-w/articles/ARvGO4zmZbSkm8RpuWuVso-w/push-down-queries-when-using-the-google-bigquery-connector-for-aws-glue?)

<br>
