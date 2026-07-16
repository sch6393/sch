Function
===

### 조회
```sql
SELECT * FROM INFORMATION_SCHEMA.ROUTINES WHERE ROUTINE_TYPE = 'FUNCTION';

SHOW FUNCTION STATUS;

# DDL 확인
SHOW CREATE FUNCTION schema_name.function_name;
```

<br>

### 기본 형식
```sql
DELIMITER $$
CREATE DEFINER=`user_name`%`host_name` FUNCTION function_name(
	p_name INTEGER 
) RETURNS VARCHAR(10) # 반환 데이터타입
BEGIN
	DECLARE v_name VARCHAR(20);

	# 데이터 처리 (Example)
	IF (pname > 10) THEN
		SET v_name = '10以上';
	ELSE
		SET v_name = '10以下';
	END IF;

	RETURN v_name;    # 반환값
END;
```

<br>

### 시퀀스 기능 작성
```sql
# 1. 시퀸스 정보를 저장할 테이블 생성
CREATE TABLE MYSQL_SEQUENCE(
    SEQUENCE_NAME VARCHAR(20) PRIMARY KEY,
    SEQUENCE_NO BIGINT NOT NULL DEFAULT 0
);

# 2. 시퀸스 정보 생성
INSERT INTO MYSQL_SEQUENCE (SEQUENCE_NAME) VALUES ('sequence1');

# 3. Function 생성
DROP FUNCTION IF EXISTS NEXTVAL; # NEXTVAL 함수가 존재하는 경우 삭제 처리

DELIMITER $$
CREATE FUNCTION NEXTVAL(
    p_sequence_name VARCHAR(20) 
) RETURNS BIGINT
BEGIN
    DECLARE v_sequence_no BIGINT;

    # MYSQL_SEQUENCE 테이블의 SEQUENCE_NO + 1
    UPDATE MYSQL_SEQUENCE SET SEQUENCE_NO = SEQUENCE_NO + 1 WHERE SEQUENCE_NAME = p_sequence_name;

    # 증가된 SEQUENCE_NO를 저장
    SELECT SEQUENCE_NO INTO v_sequence_no FROM MYSQL_SEQUENCE WHERE SEQUENCE_NAME = p_sequence_name;
    
    # 반환
    RETURN v_sequence_no;
END;

# 4. 사용법
INSERT INTO table_name(column_name, ... ) VALUES (NEXTVAL('sequence1'), ...);
```

<br>
