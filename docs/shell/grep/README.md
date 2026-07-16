grep
===

### 기본 형식
```sh
# FILE
#grep searches for PATTERNS in each FILE.
#PATTERNS is one or patterns separated by newline characters.
#And grep prints each line that matches a pattern.

# 해당 파일에 PAT 라는 글자가 포함된 부분을 출력
grep 'PAT' FILE

#grep searches for PATTERNS in each FILE.
#PATTERNS is one or patterns separated by newline characters.
```

<br>

### 옵션
```sh
# 하위 디렉토리 파일 포함
grep -r 'text' ./*

# 대소문자 구분 없이 검색
grep -i 'text' filename

# 문자열 A로 시작하여 문자열 B로 끝나는 패턴 검색
grep 'A.*B' filename

# 0-9 사이 숫자만 변경되는 패턴 검색
grep 'text[0-9]' filename

# 문자열 라인 처음 시작 패턴 검색
grep '^text' filename

# 문자열 라인 마지막 종료 패턴 검색
grep '$text' filename
```

<br>
