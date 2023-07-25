sudo
===

### 내용
명령어 앞에 `sudo` 를 붙이게 되면 관리자 권한으로 명령어가 실행됨<br>
`/etc/sudoers` 파일에 명시되어 있는 유저만 사용 가능

<br>

### 관련 에러
* `USER is not in the sudoers file. This incident will be reported.`
  ```sh
  # 1. root 접속
  su

  # 2. sudoer 파일 수정
  vi /etc/sudoers

  # 3. 아래의 root 부분을 찾고 계정 이름으로 새로 작성하여 추가
  root    ALL=(ALL)       ALL
  USER    ALL=(ALL)       ALL
  ```

<br>
