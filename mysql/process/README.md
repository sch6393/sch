Process
===

### 확인
```sql
SHOW PROCESSLIST;
/*
Id, User, Host, db, Command, Time, State, Info
9313	admin	0.0.0.0:0		Sleep	24		
9314	admin	0.0.0.0:1		Query	0	starting	show processlist
10615	rdsadmin	localhost		Sleep	2		
10970	admin	0.0.0.0:2	sys	Sleep	54		
10971	admin	0.0.0.0:3	sys	Sleep	54		
10983	user_name	0.0.0.0:4	schema_name	Sleep	687		
*/
```
>`SELECT * FROM INFORMATION_SCHEMA.PROCESSLIST;` 해당 쿼리와 같은 결과

<br>

### 프로세스 강제 종료
```sql
KILL process_id;

# RDS의 경우
CALL mysql.rds_kill(process_id);
```

<br>
