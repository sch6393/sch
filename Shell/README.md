Shell
===

### [crontab](./crontab.md)
### [iconv](./iconv.md)

<br>

### 쓰기방지 해제
```sh
chattr -i /etc/sysctl.conf
```

<br>

### 스크립트 원격 서버에서 실행
```sh
# script.sh 스크립트 파일을 원격 서버로 보내서 실행
ssh user@hostname bash < script.sh

# bash: command not found 라는 에러가 발생했을 경우
ssh user@hostname bash --login < script.sh
```

<br>

### 절대경로
```sh
abspath="$( cd "$( dirname "$0" )" && pwd -P )"
```

<br>

### 하위 디렉토리 용량 확인
```sh
du -sh ./* | sort -r
```

<br>

### 파일 내용 검색
```sh
grep -r 'text' /AAA/BBB/CCC.txt

# 하위 디렉토리 파일 포함
grep -r 'text' ./*
```

<br>

### 환경변수 확인
```sh
export
```

<br>

### 


