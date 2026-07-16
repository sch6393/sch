Patch
===

### 11.2.0.* ➞ 11.2.0.4
1. 패치 파일 실행
    ```sh
    /home/oracle/patch/database/runInstaller -jreLoc /usr/java/jdk1.8.0_301-amd64/
    ```

1. 설치 도중 발생 에러 목록
    * `checking hard limit maximum open the descriptors oracle`
      1. `/etc/security/limits.conf` 수정
          >chattr -i /etc/security/limits.conf (읽기전용해제)
      1. 내용 추가
          ```
          # Oracle 11.2.0.4 Patch
          oracle       soft    stack              10240
          oracle       hard    stack              32768

          # Ora bug 15971421
          oracle soft nproc  16384
          oracle hard nproc  16384
          oracle soft nofile 1024
          ```

    * tmp 용량이 부족할 때
      1. tmp 용량이 부족하다면 임시 폴더 지정할 필요가 있으므로 runInstaller 실행 전에 해당 환경변수를 임시적으로 등록
          * `export TEMP=/data/temp`
      1. 용량 확인
          * `swapon -s, free`
      1. Swap 파일 생성
          * `dd if=/dev/zero of=/home/swapfile_oracle bs=1024 count=100000`
      1. Swap 파일 포맷
          * `mkswap /home/swapfile_oracle`
      1. 활성화
          * `swapon /home/swapfile_oracle`
      1. 정상 진행
      1. Swap 파일 삭제
          * `swapoff /home/swapfile_oracle`
          * `rm /home/swapfile_oracle`
