crontab
===

### 기본 형식
```sh
# *  *  *  *  *
# 分 時 日 月 曜日

# 평일 11시 30분, 15시 30분에 실행
30 11,15 * * 1-5 cd /AAA/BBB CCC.sh > DDD.txt
```
>https://crontab.guru/

<br>

### crontab 확인
```sh
crontab -l

# 전체 유저의 crontab 확인
for user in $(cut -f1 -d: /etc/passwd); do echo $user; crontab -u $user -l; done
```

### crontab 수정
```sh
crontab -e
```
