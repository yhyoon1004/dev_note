# SP(Stored Procedure)

---

- 예제1

```sql
DELEMITER $$ 
--위의 DELEMEITER는 프로시저 작성이 완료되지 않았는 데 실행되는 것을 방지하기 위해
CREATE PROCEDURE 'SP_NAME'(
	--파라미터 선언
	PARAM_NAME VARCHAR(20),
	PARAM_AGE INT
)
BEGIN
	-- 변수 선언
	DECLARE PARAM_NUM INTEGER;

	--쿼리문
	SELECT COUNT(*) +1
		INTO PARAM_NUM --INTO키워드로 조회한 정보를 넣어줌
		FROM table01;
	--쿼리문
	INSERT INTO table01(tot_count, user_name, user_age) VALUES(PARAM_NUM, PARAM_NAME,PARAM_AGE);
END $$

DELEMITER ;

_________________________________
호출

CALL SP_NAME('YUNHWAN',25);
```

**파라미터 선언은 프로시저명() 안에서 선언**하고 **SQL문과 변수 선언은 BEGIN ~ END 사이**에 작성한다.

그리고 SELECT 사용 시에는 **조회한 컬럼(데이터)을 반드시 INTO로 변수 안에** 넣어줘야하며,

프로시저 내부에서 사용하는 **SQL문은 일반 SQL문이기 때문에 세미콜론(;)으로 문장을 끝맺어야한다.**

첫번째와 마지막 라인에 DELIMITER라는 이상한 단어가 있는걸 확인할 수 있는데 프로시저 작성이 완료되지 않았음에도 SQL문이 실행되는 것을 막기 위해 사용된다.

→**`DELIMITER`**는 MySQL 쿼리나 스크립트에서 SQL 문장을 구분하는 데 사용되는 구분자를 설정하는 명령

**`DELIMITER`** 명령은 이 기본 구분자를 임시로 변경하여 SQL 문장 내에서 다른 구분자를 사용할 수 있게 합니다.
**`$$`**를 사용하여 본문의 시작과 끝을 명시적으로 정의하는 것은 선택 사항입니다

---

예제2

```sql
DELIMITER $$
CREATE PROCEDURE 'SP_TEST'(
	IN loopCount1 INT,
	IN loopCount2 INT,
	OUT rst1 INT,
	OUT rst2 INT,
	INOUT rst3 INT
)
BEGIN
	DECLARE NUM1 INTEGER DEFAULT 0;
	DECLARE NUM2 INTEGER DEFAULT 0;
	DECLARE NUM3 INTEGER DEFAULT 0;

	WHILE NUM1<loopCount1 DO 
		WHILE NUM2<loopCount2 DO
			SET NUM3 = NUM3 + 1;
			SET NUM2 = NUM2 +1;
		END WHILE;

		SET NUM1 = NUM1 +1;
		SET NUM2 = 0;
	END WHILE;

	SET rst1 = NUM1;
	SET rst2 = NUM3;
	SET rst3 = rst1 + rst2 + rst3;
END $$
DELIMITER;
```