user
===

### 기본 구조
```sh
# /etc/passwd 파일 구조
user1:x:1000:2000:USERNAME:/home/user1:/bin/bash
```
|항목|설명|
|-|-|
|`user1`|계정 이름|
|`x`|`/etc/shadow` 파일에 비밀번호가 저장되어 있음. 항상 x 값이어야 함|
|`1000`|계정 UID|
|`2000`|계정의 기본 그룹에 대한 GID|
|`USERNAME`|사용자 이름|
|`/home/user1`|계정 기본 경로|
|`/bin/bash`|계정 기본 쉘|

<br>

```sh
# /etc/group 파일 구조
group1:x:2000:user1,user2,user3
```
|항목|설명|
|-|-|
|`group1`|그룹 이름|
|`x`|그룹 비밀번호. 항상 x 값이어야 함|
|`2000`|그룹 GID|
|`user1,user2,user3`|해당 그룹에 속해있는 계정 목록|

<br>

### 계정 생성
```sh
useradd -g group1 -G groupX,groupY -d /home/user1 -u 2000 -c "USERNAME" -e 2024-12-31 user1
```
>주요 그룹은 `group1`, 보조 그룹은 `groupX`, `groupY`, 기본 경로는 `/home/user1`, UID는 `2000`, 계정 이름은 `USERNAME`, 만료 날짜는 `2024-12-31`로 설정해서 계정 생성하는 커맨드

|옵션|설명|
|-|-|
|-d|계정 기본 경로 지정|
|-M|계정 기본 경로 지정하지 않음|
|-g|계정 주요 그룹 지정|
|-G|계정 보조 그룹 지정|
|-s|계정 쉘 지정|
|-u|UID 값 지정|
|-e|계정 만료 날짜 지정|
|-c|계정에 대한 간단한 정보|

### 계정 수정
```sh
usermod -s /sbin/nologin user1
```
>user1을 시스템에 로그인하지 못하도록 설정

<br>

```sh
usermod -aG groupZ user1
```
>user1에 `groupZ`를 보조 그룹으로 추가

|옵션|설명|
|-|-|
|-d|계정 기본 경로 지정|
|-m|계정 기본 경로 변경 시 기존 파일도 새 경로로 이동|
|-g|계정 주요 그룹 변경|
|-G|계정 보조 그룹 변경|
|-a|`G` 옵션과 같이 사용하는 옵션으로 보조 그룹 지정에 사용|
|-s|계정 쉘 변경|
|-u|UID 값 변경|
|-e|계정 만료 날짜 변경|
|-c|계정에 대한 간단한 정보|
|-l|계정 ID 변경|
|-L|비밀번호 잠금 (로그인 불가)|
|-U|비밀번호 잠금 해제|

<br>

### 계정 삭제
```sh
userdel -r user1
```
>기본 경로 및 메일 제거 (`/home/user1`, `/var/spool/mail/user1`)

<br>

### 계정 비밀번호 설정
```sh
passwd user1
```

<br>

### 그룹 생성
```sh
groupadd -g 1000 group1
```
>GID를 `1000`으로 그룹 생성

<br>

### 그룹 수정
```sh
groupmod -g 3000 group1
```
>GID를 `3000`으로 변경

<br>

```sh
groupmod -g group2 group1
```
>그룹 이름을 `group2`로 변경

<br>

### 그룹 삭제
```sh
groupdel group1
```

<br>

### 그룹 비밀번호 설정
```sh
gpasswd group1
```

<br>
