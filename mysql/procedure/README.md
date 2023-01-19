Procedure
===

### 기본 형식
```sql
DELIMITER $$
CREATE PROCEDURE procedure_name()
BEGIN
	DECLARE eor BOOLEAN DEFAULT FALSE;                --행 끝 여부

	--변수
	DECLARE var_name VARCHAR(2);

	--커서
	DECLARE cursor_name CURSOR FOR
		SELECT column_name FROM schema_name.table_name; --해당 쿼리 결과를 사용

	--핸들러
	DECLARE CONTINUE HANDLER                          --더 이상 행이 없다면 eor = TRUE
		FOR NOT FOUND SET eor = TRUE;

	--커서 Open
	OPEN cursor_name;
		--루프
		loop_name : LOOP
			--Fetch
			FETCH cursor_name INTO var_name;              --데이터 가져오기
			IF eor THEN
				LEAVE loop_name;                            --더 이상 데이터가 없다면 루프 해제
			END IF;

			--가져온 데이터 처리 (Example)
			/*
			IF NOT EXISTS (SELECT column_name FROM schema_name.table_name WHERE column_name = var_name)
				THEN DELETE FROM schema_name.table_name WHERE column_name = var_name;
			END IF;

			SET v_partition_name = DATE_FORMAT(DATE_ADD(DATE(v_partition_name_before), INTERVAL 1 DAY), '%Y-%m-%d');

			ALTER TABLE v_schema_name.v_table_name ADD PARTITION(
				PARTITION v_partition_name VALUES LESS THAN ( v_partition_date )
			);
			*/

		--루프 끝
		END LOOP loop_name;

	--커서 Close
	CLOSE cursor_name;
END$$
DELIMITER ;
```

<br>

### 특징
* 프로시저 정의 할때는 `DELIMITER $$` 을 사용 (문장 구분자가 `;` 인데 이는 프로시저 정의 부분에 사용해야하므로 `;` 를 인식하지 못하게 하기 위한 장치)
* 커서는 루프를 이용해 연속적인 데이터 처리를 할 수 있게 함
* FETCH는 오픈된 커서의 데이터를 한 행씩만 가져올 수 있음

<br>
