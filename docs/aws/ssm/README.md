# [Session Manager](https://ap-northeast-1.console.aws.amazon.com/systems-manager/session-manager/sessions?region=ap-northeast-1#)

### Default Setting
1. IAM Roleに `AmazonSSMManagedInstanceCore` Policyの追加が必要。
1. 以下の3つのEndpointの設定が必要。

    Endpoint|Type|Description
    -|-|-
    com.amazonaws.ap-northeast-1.s3|Gateway|S3に安全にアクセス
    com.amazonaws.ap-northeast-1.sts|Interface|IAM認証
    com.amazonaws.ap-northeast-1.ssm|Interface|SSMサービスを全体制御
    com.amazonaws.ap-northeast-1.ssmmessages|Interface|SSMのメッセージ送受信
    com.amazonaws.ap-northeast-1.ec2messages|Interface|システムとEC2のメッセージ送受信

1. 以下のSecurity Groupの設定が必要。

    SG|Inbound|Outbound
    -|-|-
    Instance用||TCP 443 該当VPC帯域
    VPC Endpoint用|All Traffic 該当VPC帯域|TCP 443 com.amazonaws.ap-northeast-1.s3

### Option
Session Logを自動的に保存、AuditしたいことであればLogを保存するS3 Bucket設定とAuditするCloudWatch設定が必要。  
設定が終わった後、SSMのPreferenceで活性化。

1. S3

    IAM Roleから以下のPolicyを追加が必要。

        {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Action": [
                        "s3:GetEncryptionConfiguration",
                        "s3:GetBucketLocation",
                        "s3:PutObject"
                    ],
                    "Resource": [
                        "arn:aws:s3:::S3-BUCKET-NAME",
                        "arn:aws:s3:::S3-BUCKET-NAME/*"
                    ]
                }
            ]
        }

1. CloudWatch
    1. 以下のEndpointの設定が必要。

        Endpoint|Type|Description
        -|-|-
        com.amazonaws.ap-northeast-1.logs|Interface|CloudWatch

    1. 以下のSecurity Groupの設定が必要。

        SG|Inbound|Outbound
        -|-|-
        Instance用||TCP 443 該当VPC帯域
        VPC Endpoint用|All Traffic 該当VPC帯域|TCP 443 com.amazonaws.ap-northeast-1.s3
