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
