crontab
===

### 기본 형식
```sh
# *  *  *  *  *
# 分 時 日 月 曜日

# 평일 11시 30분, 15시 30분에 실행
30 11,15 * * 1-5 cd /AAA/BBB; scripts.sh > text.txt
```
>https://crontab.guru/

<br>

### crontab 확인
```sh
crontab -l

# 전체 유저의 crontab 확인
for user in $(cut -f1 -d: /etc/passwd); do echo $user; crontab -u $user -l; done
```

<br>

### crontab 수정
```sh
crontab -e
```

<br>

### `bash: command not found` 라는 에러가 발생했을 경우
```sh
*/5 * * * * cd /AAA/BBB; bash --login -c 'sh script.sh'
```

<br>

### 스크립트는 정상이지만 크론 등록하면 안되는 경우
```sh
*/5 * * * * source $HOME/.bash_profile; sh /AAA/BBB/script.sh;
```
>프로파일을 못 읽어 명령어를 못 찾는 경우가 있을 수 있음 (PATH 부재)

<br>

###
