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
