Example (JavaScript, TypeScript)
===
>https://docs.aws.amazon.com/cdk/v2/guide/serverless_example.html

### 필수 조건
1. [AWS CLI](../../cli/README.md) 설치
1. [Node.js](https://nodejs.org/en) 설치
1. Node.js 설치 후 `npm install -g aws-cdk` 명령어 설치

<br>

### 순서
|순서|설명|명령어|
|-|-|-|
|1|프로젝트 디렉토리 생성|`mkdir cdk-test && cd cdk-test`|
|2|프로젝트 생성|`cdk init --language javascript`<br>`cdk init --language typescript`|
|3|CDK 라이브러리 설치|`npm install aws-cdk-lib constructs`|
|4|코드 작성|https://docs.aws.amazon.com/cdk/api/v2/docs/aws-construct-library.html|
|5|빌드 (타입스크립트만 해당)|`npm run build`|
|6|템플릿 생성|`cdk synth`<br>`cdk synth stack_name`|
|7|부트스트랩|`cdk bootstrap`|
|8|디플로이|`cdk deploy`<br>`cdk deploy stack_name`<br>`cdk deploy --all`|

<br>

### 삭제 명령어
`cdk destroy`<br>
`cdk destroy stack_name`<br>
`cdk destroy --all`

<br>

### 환경변수 설정시 주의점
>https://docs.aws.amazon.com/cdk/v2/guide/environments.html

<br>
