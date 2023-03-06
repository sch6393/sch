Timezone
===

### 확인
```sql
SHOW timezone;
```
>PostgreSQL는 보편적으로 UTC로 시간 데이터를 저장한 후, 클라이언트의 Timezone에 맞춰서 표시 (컬럼 타입이 `timestamp with time zone` 이여야 함)

