Snapshot
===

### [스냅샷 개념, 동작 원리는 해당 문서 참조](../../etc/snapshot/README.md)

<br>

### 특징
1. AWS의 스냅샷은 증분식을 사용하고 있음
1. 기본적으로 AWS에서 관리하는 S3에 스냅샷 파일이 저장됨 (유저가 해당 S3에 직접 접근은 불가능)
1. EBS와 RDS는 불가능하지만 AMI의 경우 S3로 따로 빼내 저장이 가능함
    >https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ami-store-restore.html
1. 서로 다른 AWS 계정 간의 공유가 가능하나 KMS 키가 접근 가능한 상태여야 함

<br>

### EBS의 스냅샷
>https://docs.aws.amazon.com/ebs/latest/userguide/ebs-snapshots.html

* 서로 다른 AWS 계정 간의 공유
    >https://docs.aws.amazon.com/ebs/latest/userguide/ebs-modifying-snapshot-permissions.html

<br>

### AMI
>https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html

* 서로 다른 AWS 계정 간의 공유
    >https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/sharing-amis.html

<br>

### RDS의 스냅샷
>https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateSnapshot.html

* 서로 다른 AWS 계정 간의 공유
    >https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ShareSnapshot.html

<br>

* 공유 설정 시 주의해야할 점
  1. RDS 자동 스냅샷은 공유할 수 없음 (수동으로 복사 후 가능)
  1. RDS 수동 스냅샷은 KMS 변경이 되지 않음
  1. RDS 성능 개선 도우미가 활성되어있으면 KMS 변경이 되지 않음
  1. Default KMS로 생성되는 RDS 스냅샷은 외부 공유가 불가능

<br>
