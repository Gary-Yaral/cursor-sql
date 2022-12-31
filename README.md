# CURSOR AND PROCEDURES - MYSQL
## Cómo insertar datos en una tabla usando los resultados de una consulta previa

```sql
DROP PROCEDURE IF EXISTS insertStudent;

DELIMITER $$
CREATE PROCEDURE insertStudent (dni VARCHAR(64), stdname VARCHAR(100), lastname VARCHAR(100), course INT)
BEGIN
	DECLARE idInput INTEGER;
	DECLARE finished BOOLEAN DEFAULT FALSE;
	DECLARE cursor1 CURSOR FOR 
		SELECT id FROM input WHERE course_id = course;

	-- declare NOT FOUND handler
	DECLARE CONTINUE HANDLER 
        FOR NOT FOUND SET finished = TRUE;
        
	INSERT INTO student(dni, `name`, lastname, course_id) VALUES(dni, stdname, lastname, course);
	SET @stdID = (SELECT id FROM student WHERE dni = dni AND course_id = course);
	
	OPEN cursor1;
	loop_1: LOOP
		FETCH cursor1 INTO idInput;
		IF finished = TRUE THEN 
			LEAVE loop_1;
		END IF;
		INSERT INTO input_student(input, student, qualification) VALUES(idInput, @stdID, 0);

	END LOOP loop_1;
	CLOSE cursor1;

END$$
```
