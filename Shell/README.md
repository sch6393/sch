Shell
===

### 쓰기방지 해제
```sh
chattr -i /etc/sysctl.conf
```

### 스크립트 원격 서버에서 실행
```sh
# script.sh 스크립트 파일을 원격 서버로 보내서 실행
ssh user@hostname bash < script.sh

# bash: command not found 라는 에러가 발생했을 경우
ssh user@hostname bash --login < script.sh
```

<br>

### [crontab](./crontab.md)
