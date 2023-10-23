SQL Mode
===
>https://dev.mysql.com/doc/refman/8.0/en/sql-mode.html

### 설정
```sql
SET GLOBAL sql_mode='mode_name';
SET SESSION sql_mode='mode_name';
```

<br>

### 종류
|목록|설명|
|-|-|
|`ONLY_FULL_GROUP_BY`|`SELECT`나 HAVING 조건 또는 ORDER BY 목록이 GROUP BY에 이름이 지정되지 않거나 GROUP BY의 컬럼에 PK나 UK인 컬럼을 참조할 경우 쿼리 실행을 막음|
|`STRICT_TRANS_TABLES`|트랜잭션 스토리지 엔진에 대해 [Strict SQL Mode](#strict-sql-mode)를 활성화하고 가능한 경우 비트랜잭션 스토리지 엔진을 활성화|
|`NO_ZERO_IN_DATE`<br>(8.0부터 Deprecated)|년 또는 일에 0 값을 허용하는지에 대한 설정<br><br>비활성화 : 0이 포함된 날짜가 허용되고 INSERT 시 경고 없음<br>활성화 : 0이 포함된 날짜는 0000-00-00으로 INSERT되고 경고 발생<br>Strict 모드와 같이 활성화  : 0이 포함된 날짜는 허용되지 않고 INSERT, UPDATE 시 에러 발생 (단, IGNORE 옵션이 있다면 0000-00-00으로 INSERT, UPDATE되고 경고 발생)|
|`NO_ZERO_DATE`<br>(8.0부터 Deprecated)|0000-00-00을 유효한 날짜로 허용하는지에 대한 설정<br><br>비활성화 : 0000-00-00이 허용되고 INSERT 시 경고 없음<br>활성화 : 0000-00-00이 허용되고 INSERT 시 경고 발생<br>Strict 모드와 같이 활성화  : 0000-00-00이 허용되지 않고 INSERT, UPDATE 시 에러 발생 (단, IGNORE 옵션이 있다면 0000-00-00으로 INSERT, UPDATE되고 경고 발생)|
|`ERROR_FOR_DIVISION_BY_ZERO`<br>(8.0부터 Deprecated)|0으로 나눌 때의 처리 설정<br><br>비활성화 : INSERT 시 NULL 값이 들어가고 경고 없음<br>활성화 : INSERT 시 NULL 값이 들어가고 경고 발생<br>Strict 모드와 같이 활성화  : SELECT의 경우는 NULL 반환, INSERT, UPDATE 시에는 에러 발생 (단, IGNORE 옵션이 있다면 NULL로 INSERT, UPDATE되고 경고 발생)|
|`NO_ENGINE_SUBSTITUITION`|CREATE TABLE, ALTER TABLE 쿼리를 비활성화하거나 컴파일 되지 않은 스토리지 엔진을 지정할 시 기본 스토리지 엔진으로 자동으로 대체할 것 인지를 설정<br><br>비활성화 : CREATE TABLE 실행 시 기본 엔진이 선택되며 지정한 엔진을 사용할 수 없다면 경고 발생, ALTER TABLE은 변경되지 않고 경고 발생<br>활성화 : 지정한 엔진을 사용할 수 없는 경우 에러가 발생되고 생성 및 변경되지 않음|

<br>

### Strict SQL Mode
* Strict SQL Mode는 INSERT나 UPDATE처럼 데이터 변경 문법에서 유효하지 않거나 없는 값을 어떻게 MySQL에서 처리할 것인지를 설정. DDL 문법에도 영향이 있음
  * 예) 명시적으로 DEFAULT가 정의되어 있지 않은 NOT NULL 컬럼에 대해 INSERT를 할 때 값이 없다면 누락됨 (NOT NULL 컬럼이 아니라면 NULL이 INSERT 됨)

<br>
