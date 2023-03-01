S3
===
>[https://aws.amazon.com/s3/](https://aws.amazon.com/s3/)
>[https://docs.aws.amazon.com/cli/latest/userguide/cli-services-s3.html](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-s3.html)
>[https://docs.aws.amazon.com/cli/latest/reference/s3/](https://docs.aws.amazon.com/cli/latest/reference/s3/)

### 기본 명령어 형식
* `aws s3 `[cmd](#cmd)` file_name s3://s3_bucket_name/`

<br>

### cmd
|구분|명령어|
|-|-|
|단일 파일, S3 객체|`cp`<br>`mv`<br>`rm`|
|디렉토리|`sync`<br>`mb`<br>`rb`<br>`ls`|

<br>

### `fatal error: An error occurred (404) when calling the HeadObject operation: Key "" does not exist` 에러가 발생할 경우 (대부분 디렉토리를 작업하려고 할때 발생)
* `--recursive` 옵션 추가
    ```sh
    aws s3 cp directory_name s3://s3_bucket_name/ --recursive
    ```

<br>

### 해당 버킷의 파일 리스트 뽑기
```sh
# json
aws s3api list-objects-v2 --bucket BUCKET_NAME > FILENAME.json

# csv
aws s3api list-objects-v2 --bucket BUCKET_NAME | jq -r '.[] | .[] | [.Key, .LastModified, .Size] | @csv' > FILENAME.csv
```

* 목록
  |Type|Description|
  |-|-|
  |`Key`|파일 디렉토리 + 이름|
  |`LastModified`|마지막 수정 날짜|
  |`ETag`|HTTP의 Response Header이며 리소스에 대한 특정 버전의 식별자. URL에 있는 리소스에 어떠한 변화가 있다면 ETag도 값이 새로 바뀜<br>[https://docs.aws.amazon.com/AmazonS3/latest/API/RESTCommonResponseHeaders.html](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTCommonResponseHeaders.html)|
  |`Size`|파일 크기 (bytes)|
  |`StorageClass`|[스토리지 클래스](#스토리지-클래스)|
  |`Owner`|파일 소유자|

* json
  ```json
  {
    "Contents": [
        {
            "Key": "sql.sql",
            "LastModified": "2023-02-26T08:04:20.000Z",
            "ETag": "\"d41d8cd98f00b204e9800998ecf8427e\"",
            "Size": 442368,
            "StorageClass": "STANDARD"
        },
        {
            "Key": "glacier-da/dumpfile.dmp",
            "LastModified": "2023-02-22T05:26:59.000Z",
            "ETag": "\"67de4b3812fa193ffdb3258b7d778c7c-7892\"",
            "Size": 13665677312,
            "StorageClass": "DEEP_ARCHIVE"
        },
        ...
    ]
  }
  ```

<br>

### 스토리지 클래스
>https://aws.amazon.com/s3/storage-classes/?nc1=h_ls

|스토리지 클래스|다음 용도로 설계됨|가용 영역|최소 스토리지 기간|청구 가능한 최소 객체 크기|모니터링 및 자동 계층화 요금|검색 요금|
|-|-|-|-|-|-|-|
|스탠다드|자주 액세스하는 데이터 (한 달에 한 번 이상) (밀리초 단위로 액세스)|≥ 3|||||
|지능형 계층화|액세스 패턴이 바뀌거나 알 수 없는 데이터|≥ 3|||128KB 이상인 객체에는 객체당 요금이 적용||
|Standard-IA|자주 액세스하지 않는 데이터 (한 달에 한 번) (밀리초 단위로 액세스)|≥ 3|30일|128KB||GB당 요금 적용|
|One Zone-IA|단일 가용 영역에 저장되는 재생성 가능하고 자주 액세스하지 않는 (한 달에 한 번) 데이터 (밀리초 단위로 액세스)|1|30일|128KB||GB당 요금 적용|
|Glacier Instant Retrieval|분기에 한 번 액세스하는 오래된 아카이브 데이터 (밀리초 내에 즉시 검색)|≥ 3|90일|128KB||GB당 요금 적용|
|Glacier Flexible Retrieval (이전 명칭: Glacier)|일 년에 한 번 액세스하는 오래된 아카이브 데이터 (몇 분 내지 몇 시간의 검색 시간 소요)|≥ 3|90일|||GB당 요금 적용|
|Glacier Deep Archive|일 년에 한 번 미만으로 액세스하는 오래된 아카이브 데이터 (몇 시간의 검색 시간 소요)|≥ 3|180일|||GB당 요금 적용|
|중복 감소|중요하지 않고 자주 액세스하는 데이터 (밀리초 단위로 액세스) (S3 Standard가 더 비용 효율적이므로 권장되지 않음)|≥ 3||||GB당 요금 적용|

<br>
