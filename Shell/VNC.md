VNC
===

### 내용
```sh

```

<br>

### VNC 명령어
```sh
# vnc 실행
vncserver

# vnc 리스트 확인
vncserver -list

# vnc 종료
vncserver -kill display_number
```

<br>

### GUI 프로그램 실행 시 `No protocol specified` 라는 에러가 발생할 경우
```sh
# root 계정에서
xhost +
# access control disabled, clients can connect from any host
```

<br>
