Install
===
>[https://www.postgresql.org/download/](https://www.postgresql.org/download/)

### dnf 설치, 제거
```sh
# 공식 저장소를 추가
dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm

# 내장 모듈을 비활성화
dnf -qy module disable postgresql

# 위의 절차를 권장함

# Install
dnf install postgresql16-server

# Remove
dnf remove postgresql16-server
```

<br>

### 초기 설정
```sh
# Init
/usr/pgsql-16/bin/postgresql-16-setup initdb

# 서비스 등록, 자동 실행
systemctl enable postgresql-16
systemctl start postgresql-16

# 상태확인
systemctl status postgresql-16

# 재시작
systemctl restart postgresql-16

# 설정파일
/var/lib/pgsql/16/data/postgresql.conf
/var/lib/pgsql/16/data/pg_hba.conf
```


