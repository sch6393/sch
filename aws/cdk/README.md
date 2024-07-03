CDK
===
>https://docs.aws.amazon.com/cdk/v2/guide/home.html

### AWS Cloud Development Kit
코드 및 클라우드 포메이션 (CloudFormation) 을 통해 프로비저닝하고 인프라를 개발 언어로 관리할 수 있게 함

<br>

### 구조
<img src="AppStacks.png">

>https://docs.aws.amazon.com/cdk/v2/guide/home.html

1. ***App*** : CDK CLI를 통해서 CloudFormation 템플릿을 렌더링, 배포함. ***App***은 리전 및 계정에 대한 정보가 포함된 하나의 **Stack**으로 구성됨
    * `cdk depoly` 명령어로 AWS CDK가 배포됨
    * 이하와 같은 과정을 거침
      1. Construction (구축) : 앞에서 정의한 모든 *Construct*를 인스턴스화 하며 연결함. 대부분의 코드가 해당 단계에서 실행
      1. Preparation (준비) : *Construct* 트리 생성
      1. Validation (검증) : 배포가 정상적인 상태인지 확인하며 실패할 경우 알림이 발생
      1. Synthesis (종합) : 실행 단계로 `app.synth()` 에서 호출됨. *Construct* 트리를 순회하며 모든 *Construct*에 대해 synthesize 메서드를 호출함. 해당 단계에서 클라우드 포메이션 템플릿, 람다 애플리케이션 번들, 도커 이미지 등 배포를 위해 필요한 구성 요소들이 작성됨
      1. Deployment (배포) : AWS 환경에 배포. 구성 요소들은 S3와 ECR (Elastic Container Repository) 에 업로드 되고 클라우드 포메이션으로 애플리케이션이 배포되며 리소스를 생성


1. **Stack** : CDK에서 배포하는 단위. 모든 AWS 리소스는 **Stack**의 범위와 단위 내에서 프로비저닝됨. **Stack**은 *Construct*로 구성됨
    * `cdk ls` 명령어로 ***App*** 안의 **Stack** 리스트를 볼 수 있음
    * `cdk synth` 명령어로 템플릿을 생성함 (**Stack** 이름을 지정해야 각각의 **Stack**에 대해 템플릿 생성이 가능함)

1. *Construct* : CDK 앱을 만들기 위한 기본 블록. *Construct*가 인스턴스화 되었다면 다른 *Construct*와 상호작용이 가능함. *Construct*는 3개의 파라미터를 가짐
    1. Scope : *Construct*가 정의되는 범위
    1. Id : Scope 안에서의 유일한 식별자이며 Scope의 하위 트리 내에 캡슐화된 모든 리소스에 대한 네임 스페이스 역할 (말이 어렵지만 리소스 이름과 같은 중복될 수 없는 값을 할당할 때 사용됨)
    1. Props : 리소스 구성을 정의하는 속성이나 키워드의 집합

<br>

### AWS Document
>https://docs.aws.amazon.com/cdk/api/v2/

>https://github.com/aws-samples/aws-cdk-examples

<br>

### IaC (Infrastructure as Code)
1. 일관성 : 코드로 인프라 구성이 가능하기 때문에 반복적인 인프라 작업의 자동화가 가능
1. 효율성 : 코드의 재사용성과 모듈화 가능. 또한 버전 관리나 변경 내용 추적, 빠른 배포 및 롤백 가능
1. 안정성 : 테스트와 검증이 자동화되어 있기 때문에 에러 감지, 방지 가능

<br>

### 다른 IaC 서비스 툴
1. Terraform
1. CloudFormation

<br>
