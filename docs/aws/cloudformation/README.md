CloudFormation
===
>https://aws.amazon.com/cloudformation/?nc1=h_ls

>https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html

### [IaC (Infrastructure as Code)](../aws/cdk/README.md#iac-infrastructure-as-code)
해당 문서 참조

<br>

### 구조
1. `JSON`
    ```json
    "Ec2Instance": {
        "Type": "AWS::EC2::Instance",
        "Properties": {
            "AvailabilityZone": "aa-example-1a",
            "ImageId": "ami-1234567890abcdef0"
        }
    }
    ```

1. `YAML`
    ```yaml
    Ec2Instance:
        Type: AWS::EC2::Instance
        Properties:
            AvailabilityZone: aa-example-1a
            ImageId: ami-1234567890abcdef0
    ```

<br>

### 입력 파라미터를 이용할 경우
>https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/gettingstarted.templatebasics.html#gettingstarted.templatebasics.parameters

<br>

### 특정 파라미터를 매핑할 경우
>https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/gettingstarted.templatebasics.html#gettingstarted.templatebasics.mappings

<br>
