CLI
===

### 설치
* [https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

<br>

### 인증정보 설정
* `aws configure` 명령어로 인증정보 입력
    ```
    AWS Access Key ID : 
    AWS Secret Access Key : 
    Default region name : 
    Default output format : 
    ```

* `aws configure --profile profile_name` 명령어로 프로파일 설정
  * 프로파일 전환
    ```sh
    export AWS_PROFILE=profile_name

    # 윈도우의 경우
    set AWS_PROFILE=profile_name
    ```
  * 프로파일 확인
    ```sh
    aws configure list
    ```
    

<br>

### [인증정보 확인](../iam/README.md)
AWS Console ➞ IAM ➞ 사용자 ➞ 보안 자격 증명 ➞ 액세스 키

<br>
