git
===

### git config
```
git config --global user.name "Son ChangHan"
git config --global user.email sch6393@gmail.com

git config --global commit.template .gitmessage.txt
```

<br>

### [gitmessage.txt](./gitmessage.txt.md)

<br>

### git commit
```
git add .
git commmit -m "Modify"
git push origin main

git pull
```

<br>

### git branch
```
git branch
git checkout branch_name
git merge branch_name
```

<br>

### git CRLF, LF
|명령어|설명|
|-|-|
|`git config –global core.autocrlf true`|커밋할 시 CRLF ➞ LF, 체크아웃 시 LF ➞ CRLF (윈도우 전용)|
|`git config –global core.autocrlf input`|커밋할 시 CRLF ➞ LF, (리눅스, 맥)|
|`git config –global core.autocrlf false`|자동변환 없음 (기본 값)|

<br>

### git remote
```
git remote set-url origin https://github.com/sch6393/SCH.git
```

<br>
