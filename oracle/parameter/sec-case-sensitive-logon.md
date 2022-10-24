sec_case_sensitive_logon
===
>비밀번호 대소문자 구분 여부

### 확인
```sql
SHOW PARAMETER sec_case;
/*
TRUE  : 구분 O
FALSE : 구분 X
*/
```

<br>

### 변경
```sql
ALTER SYSTEM SET sec_case_sensitive_logon=FALSE;
```

<br>
