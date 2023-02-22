timedatectl
===

### 기본 형식
```sh
timedatectl

#      Local time: Wed 2023-02-22 10:58:15 JST
#  Universal time: Wed 2023-02-22 01:58:15 UTC
#        RTC time: Wed 2023-02-22 01:58:16
#       Time zone: Asia/Tokyo (JST, +0900)
#     NTP enabled: yes
#NTP synchronized: yes
# RTC in local TZ: no
#      DST active: n/a
```

<br>

### 타임존 변경
```sh
timedatectl set-timezone Asia/Seoul

# 타임존 리스트 확인
timedatectl list-timezones
```

<br>
