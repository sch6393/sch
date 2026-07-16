RDS
===

### [Aurora](./aurora/README.md)
### [MySQL](./mysql/README.md)
### [Oracle](./oracle/README.md)
### [PostgreSQL](./postgresql/README.md)

### 스냅샷 공유
>https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ShareSnapshot.html

1. 「A」 계정에서 KMS 고객 관리형 키 생성
1. 키의 관리권한과 사용권한을 정의하고 「B」 계정 ID를 입력
1. 「A」 계정에서 스냅샷 복사를 통해 수동 스냅샷을 생성 (KMS 키를 위에서 생성했던 키로 지정)
1. 생성한 스냅샷을 공유
1. 「B」 계정에서 공유 받은 스냅샷으로 스냅샷 복사 (옵션 그룹을 지정해줘야 하고 KMS 키는 위에서 생성했던 키로 지정)
1. 복사한 스냅샷으로 RDS 인스턴스 생성

<br>
