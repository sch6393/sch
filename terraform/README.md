Terraform
===
>https://www.terraform.io/

### [IaC (Infrastructure as Code)](../aws/cdk/README.md#iac-infrastructure-as-code)
해당 문서 참조

<br>

### 테라폼의 구조
1. 프로비저닝 (Provisioning)

    어떤 프로세스나 서비스를 실행하기 위한 준비 단계. 프로비저닝에는 크게 네트워크나 컴퓨팅 자원을 준비하는 작업과 준비된 컴퓨팅 자원에 사이트 패키지나 애플리케이션 의존성을 준비하는 단계로 나뉘는데 테라폼은 전자에 해당

1. 프로바이더 (Provider)

    테라폼과 외부 서비스를 연결해주는 기능을 하는 모듈. 예를 들어 AWS, 구글 클라우드 플랫폼 (Google Cloud Platform), Microsoft Azure와 같은 범용 클라우드 서비스를 포함해 깃허브와 같은 특정 기능을 제공하는 서비스, MySQL, Docker와 같은 로컬 서비스 등을 지원

1. 리소스(자원)Resource
    리소스란 특정 프로바이더가 제공하는 조작 가능한 대상의 최소 단위. 예를 들어 AWS 프로바이더라면 VPC나 EC2 인스턴스, RDS, 키 페어 등이 AWS 프로바이더가 제공해주는 리소스 타입에 해당

1. HCL (Hashicorp Configuration Language)

    HCL은 테라폼에서 사용하는 설정 언어. 테라폼에서 모든 설정과 리소스 선언은 HCL을 사용함. 확장자는 `.tf`

1. 계획 (Plan)

    테라폼 프로젝트 디렉터리 아래의 모든 `.tf` 파일의 내용을 실제로 적용 가능한지 확인하는 작업. 어떤 리소스가 생성, 수정, 삭제될지 계획을 표시

1. 적용 (Apply)

    테라폼 프로젝트 디렉터리 아래의 모든 `.tf` 파일의 내용대로 리소스를 생성, 수정, 삭제하는 작업. 변경 전에도 Plan의 결과로 확인 가능

<br>

### 시작
* [Terraform](https://developer.hashicorp.com/terraform/install?product_intent=terraform) 다운로드 후 환경변수 설정

<br>

### 순서
1. 프로젝트 디렉토리 생성
    ```
    mkdir terraform-test && cd terraform-test
    ```

1. `main.tf` 파일 생성 및 아래의 테스트 내용 작성
    ```tf
    # Provider (인프라 정보)
    provider "aws" {
      # profile이 있다면 입력. 없다면 ./aws/credentials 의 정보를 이용함
      profile = "sch6393"
      region = "us-east-1"
    }

    # Resource (인프라 자원)
    # test-vpc 이라는 이름을 가지는 VPC를 생성
    resource "aws_vpc" "test-vpc" {
      cidr_block = "10.0.0.0/16"

      tags = {
        Name = "test-vpc"
      }
    }

    # Public Subnet
    resource "aws_subnet" "test-subnet-public1-1a" {
      vpc_id     = aws_vpc.test-vpc.id
      cidr_block = "10.0.0.0/24"
      availability_zone = "us-east-1a"

      tags = {
        Name = "test-subnet-public1-1a"
      }
    }

    resource "aws_subnet" "test-subnet-public2-1c" {
      vpc_id     = aws_vpc.test-vpc.id
      cidr_block = "10.0.1.0/24"
      availability_zone = "us-east-1c"

      tags = {
        Name = "test-subnet-public2-1c"
      }
    }

    # Private
    resource "aws_subnet" "test-subnet-private1-1a" {
      vpc_id     = aws_vpc.test-vpc.id
      cidr_block = "10.0.2.0/24"
      availability_zone = "us-east-1a"

      tags = {
        Name = "test-subnet-private1-1a"
      }
    }

    resource "aws_subnet" "test-subnet-private2-1c" {
      vpc_id     = aws_vpc.test-vpc.id
      cidr_block = "10.0.3.0/24"
      availability_zone = "us-east-1c"

      tags = {
        Name = "test-subnet-private2-1c"
      }
    }
    ```

1. 프로젝트 생성
    ```
    terraform init
    ```

1. 테라폼의 인프라 설정과 현재 적용하려는 실제 인프라의 비교
    ```
    terraform plan
    ```

1. 변경내용 적용
    ```
    terraform apply
    ```

<br>

### 삭제
```
terraform destroy
```

<br>

### 리소스 및 Example 코드 확인
>https://registry.terraform.io/

<br>
