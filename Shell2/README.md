Shell
===

### [crontab](./crontab/README.md)
### [iconv](./iconv/README.md)
### [iostat](./iostat/README.md)
### [sar](./sar/README.md)
### [top](./top/README.md)
### [VNC](./VNC/README.md)

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

### 파일 내용 치환
```sh
# AAA ➞ BBB
sed 's/AAA/BBB/g' test.txt
```

<br>

### 파일 이름 검색
```sh
find . -name "*.sh"
```

<br>

### 환경변수 확인
```sh
export
```

<br>

### zip 파일 압축 풀기
```sh
unzip file_name.zip
```

<br>

### Timezone
```sh
# 시간 정보 표시
timedatectl

# 타임존 변경
timedatectl set-timezone Asia/Tokyo
```

<br>
