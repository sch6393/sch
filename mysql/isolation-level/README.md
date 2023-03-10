Isolation Level
===
>[해당 문서 참고](../../etc/isolation-level/README.md)

### 확인
```sql
SHOW VARIABLES LIKE 'transaction_isolation';
/*
Variable_name, Value
transaction_isolation	REPEATABLE-READ
*/
```
>MySQL InnoDB는 `REPEATABLE-READ`가 기본 격리 수준 레벨로 설정되어 있음

<br>
