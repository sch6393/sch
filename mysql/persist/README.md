Persist
===

### 내용
* MySQL의 경우 파라미터를 수정을 영구적으로 반영하려면 my.cnf에도 해당 내용을 적어야 하는데 이를 잊어먹는 경우가 있음
* 반대로 my.cnf에 해당 내용을 적어놓고 실제 인스턴스에는 반영을 잊어먹는 경우가 있음
* 위의 상황이 일어나고 인스턴스가 재기동이 된다면 파라미터가 리셋 또는 멋대로 적용되어 문제가 발생하기 때문에 인스턴스에 적용과 동시에 mysqld-auto.cnf 파일도 같이 바뀔 수 있도록 해당 구문이 도입됨
* mysqld-auto.cnf 파일에서 변경된 시간과 변경한 계정을 알 수 있음
* 해당 구문은 `8.0` 부터 사용 가능

<br>

### 사용법
```sql
# SET GLOBAL + mysqld-auto.cnf 파일 작성
SET PERSIST variable_name = value;

# mysqld-auto.cnf 파일 작성
SET PERSIST_ONLY variable_name = value;
```

<br>

### 관련 내용
* [mysqld](../mysqld/README.md)

<br>
