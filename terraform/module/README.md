Terraform Module
===
>https://registry.terraform.io/browse/modules

>https://github.com/terraform-aws-modules

### 특징
1. 캡슐화

    서로 관련 있는 오브젝트들끼리 묶어 효율화

1. 재사용성

    모듈을 사용해 리소스를 작성을 하기 때문에 다른 환경에도 쉽게 재사용이 가능

1. 일관성

    재사용성으로 인해 에러를 줄이고 일관성을 가지게 됨

<br>

### 기본구조
```
module "ID_NAME" {
    source = "SOURCE_NAME"

    [CONFIG_NAME = ...]
}
```
|키워드|설명|
|-|-|
|`ID_NAME`|테라폼 코드 전체에서 사용하는 유일한 식별자|
|`SOURCE_NAME`|모듈 코드에서 찾을 수 있는 디렉토리 (Terraform 클라우드 URL도 가능)|
|`CONFIG_NAME`|모듈과 관련된 인수|
>Terraform 클라우드 ➞ https://app.terraform.io/session

<br>

### 예시1
* 디렉토리 구조
  ```
  terraform-project
  ├ module
  │ ├ vpc
  │ │ ├ vpc.tf
  │ │ ├ variable.tf
  │ │ └ output.tf
  │ ├ sg
  │ │ └ …
  │ ├ ec2
  │ │ └ …
  │ …
  │
  └ main.tf
  ```

* `main.tf` - 각 모듈을 호출하는 부분
  ```
  # VPC Module 호출
  module "vpc" {
    source = "./module/vpc"
    vpc_cidr_block    = "10.0.0.0/16"
    subnet_cidr_block = "10.0.1.0/24"
  }
  ```

* `vpc.tf` - 리소스를 정의하는 부분
  ```
  resource "aws_vpc" "test" {
    cidr_block = var.vpc_cidr_block
    # ...
  }

  resource "aws_subnet" "test" {
    vpc_id     = aws_vpc.test.id
    cidr_block = var.subnet_cidr_block
    # ...
  }
  ```

* `variable.tf` - 리소스에서 사용할 변수 정의 부분
  ```
  variable "vpc_cidr_block" {
    description = "The CIDR block for the VPC."
    type        = string
  }

  variable "subnet_cidr_block" {
    description = "The CIDR block for the subnet."
    type        = string
  }
  ```

* `output.tf` - 출력값 정의 부분
  ```
  output "vpc_id" {
    value = aws_vpc.test.id
  }

  output "subnet_id" {
    value = aws_subnet.test.id
  }
  ```

<br>

### 예시2 (외부 라이브러리 사용)
* 디렉토리 구조
  ```
  terraform-project
  ├ modules
  │ ├ cluster
  │ │ └ https://github.com/terraform-aws-modules/terraform-aws-rds
  │ ├ vpc
  │ │ └ https://github.com/terraform-aws-modules/terraform-aws-vpc
  │ …
  │
  └ main.tf
  ```
  >modules 안의 폴더는 https://github.com/terraform-aws-modules 의 라이브러리를 따름

* `main.tf` - 각 모듈을 호출하는 부분
  ```
  # Module 호출
  module "vpc" {
    source = "terraform-aws-modules/vpc/aws"
    #vpc_cidr_block    = "10.0.0.0/16"
    #subnet_cidr_block = "10.0.1.0/24"
    name = "test-vpc"
    cidr = "10.0.0.0/16"

    azs             = ["eu-east-1a", "eu-east-1b", "eu-east-1c"]
    private_subnets = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
    public_subnets  = ["10.0.101.0/24", "10.0.102.0/24", "10.0.103.0/24"]

    #enable_nat_gateway = true
    #enable_vpn_gateway = true

    tags = {
        Terraform = "true"
        Environment = "dev"
    }
  }

  module "cluster" {
    source  = "terraform-aws-modules/rds-aurora/aws"

    name           = "test-aurora-db-postgres"
    engine         = "aurora-postgresql"
    engine_version = "14.5"
    instance_class = "db.r6g.large"
    instances = {
      one = {}
      2 = {
        instance_class = "db.r6g.2xlarge"
      }
    }

    vpc_id               = "vpc-12345678"
    db_subnet_group_name = "db-subnet-group"
    security_group_rules = {
      ex1_ingress = {
        cidr_blocks = ["10.20.0.0/20"]
      }
      ex1_ingress = {
        source_security_group_id = "sg-12345678"
      }
    }

    storage_encrypted   = true
    apply_immediately   = true
    monitoring_interval = 10

    enabled_cloudwatch_logs_exports = ["postgresql"]

    tags = {
      Environment = "dev"
      Terraform   = "true"
    }
  }
  ```

<br>
