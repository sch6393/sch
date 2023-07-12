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

### 관련 파라미터 확인
```sql
SHOW VARIABLES WHERE VARIABLE_NAME LIKE '%connections';
/*
max_connections	2000         현재 설정되어 있는 최대 동시 연결 수
max_user_connections	0    계정별 생성 가능한 최대 동시 연결 수
*/
```

<br>

### 관련 STATUS 확인
```sql
SHOW STATUS WHERE VARIABLE_NAME IN (
    'aborted_clients', 'aborted_connects', 'connections', 'max_used_connections', 'threads_connected',
);
/*
Aborted_clients	0               연결된 상태에서 강제로 연결이 끊긴 수
Aborted_connects	0           연결 과정 중에 실패한 수
Connections	854086              총 연결 시도 회수
Max_used_connections	797     최대로 접속했던 연결 수
Threads_connected	677         현재 연결되어 있는 수
*/
```

<br>
