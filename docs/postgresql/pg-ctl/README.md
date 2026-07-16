pg_ctl
===
>https://www.postgresql.org/docs/current/app-pg-ctl.html

### 기본
```sh
pg_ctl start                        # 시작
pg_ctl stop                         # 정지
pg_ctl restart                      # 재시작
pg_ctl reload                       # 로드
pg_ctl status                       # 상태
pg_ctl kill PID                     # 프로세스 정지
pg_ctl register -N service-name     # 서비스 등록
pg_ctl unregister -N service-name   # 서비스 등록 해제
```

<br>

### 옵션
|옵션|설명|
|-|-|
|`-D`|PostgreSQL 데이터 디렉토리 지정|
|`-l`|로그 파일 지정|
|`-m`|`smart` : 모든 클라이언트의 연결이 끊기면 종료<br>`fast` : 클라이언트의 연결을 강제로 끊고 종료<br>`immediate` : 강제 종료 (복구 작업이 발생할 가능성 있음)|
|`-s`|로그 출력을 하지 않음 (에러만 표시)|
|`-t`|작업 완료까지의 대기시간 설정 (초단위)|
|`-w`|작업 완료까지 기다림|
|`-W`|작업 완료까지 기다리지 않음|

<br>
