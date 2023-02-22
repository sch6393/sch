find
===

### 기본 형식
```sh
find "filename"
```

<br>

### 옵션
```sh
# 타입 지정 (f : 파일, d : 디렉토리, l : 심볼릭 링크)
find . -type f

# 이름 지정
find . -name "*.sh"

# 10MB 인 파일 검색
find . -type f -size 10M

# 100MB ~ 1024MB 사이인 파일 검색
find . -type f -size +100M -size -1024M

# 특정 파일 이후에 생성된 파일 검색
find . -type f -newer filename

# 액세스 시점으로 파일 검색
# 7일 동안 액세스 되지 않은 파일
find . -type f -atime +7
# 7일 동안 액세스 됐던 파일
find . -type f -atime -7

# 수정된 시점으로 파일 검색
# 7일 동안 수정 되지 않은 파일
find . -type f -mtime +7
# 7일 동안 수정 됐던 파일
find . -type f -mtime -7

# 생성된 시점으로 파일 검색
# 생성된 지 7일이 넘은 파일
find . -type f -ctime +7
# 생성된 지 7일이 되지 않은 파일
find . -type f -ctime -7
```

<br>
