ORA-39002: invalid operation, ORA-39070: Unable to open the log file., ORA-29283: invalid file operation, ORA-06512: at "SYS.UTL_FILE", line 536, ORA-29283: invalid file operation
===
>expdp, impdp시 해당 에러가 발생한 경우

1. 덤프 디렉토리에 오라클 계정이 액세스 가능한지 확인
    ```sh
    chown -R 커맨드로 권한 변경
    ```
