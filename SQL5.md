# Stepik SQL

```
SELECT CONCAT(LEFT(CONCAT(module_id, ' ', module_name), 16), '...') Модуль,
       CONCAT(LEFT(CONCAT(module_id, '.', lesson_position, ' ', lesson_name), 16), '...') Урок,
       CONCAT(module_id, '.', lesson_position, '.', step_position, ' ', step_name) Шаг
  FROM module
       INNER JOIN lesson USING(module_id)
       INNER JOIN step   USING(lesson_id)
 WHERE step_name LIKE '%ложенн% запрос%'
 ORDER BY module_id, lesson_id, step_id;
```


```
INSERT INTO step_keyword
SELECT step.step_id, keyword.keyword_id 
FROM 
    keyword
    CROSS JOIN step
WHERE step.step_name REGEXP CONCAT(' ', CONCAT(keyword.keyword_name, '\\b'))
GROUP BY step.step_id, keyword.keyword_id
ORDER BY keyword.keyword_id;
```

```
SELECT 
    concat(module_id,'.',lesson_position,
           IF(step_position < 10, ".0","."),
           step_position," ",step_name) AS Шаг
FROM
   step
   JOIN lesson USING(lesson_id)
   JOIN module USING(module_id)
   JOIN step_keyword USING (step_id)
   JOIN keyword USING(keyword_id)
WHERE keyword_name = 'MAX' OR keyword_name ='AVG'
GROUP BY ШАГ
HAVING COUNT(*) = 2
ORDER BY 1;
```


```
SELECT
    rate_group Группа, 
    CASE rate_group
        WHEN 'I'   THEN 'от 0 до 10'
        WHEN 'II'  THEN 'от 11 до 15'
        WHEN 'III' THEN 'от 16 до 27'
        ELSE 'больше 27'
    END Интервал,
    COUNT(*) Количество
FROM
(
    SELECT 
        CASE
            WHEN COUNT(DISTINCT step_id) <= 10 THEN 'I'
            WHEN COUNT(DISTINCT step_id) <= 15 THEN 'II'
            WHEN COUNT(DISTINCT step_id) <= 27 THEN 'III'
            ELSE 'IV'
        END rate_group
    FROM step_student
    WHERE result = 'correct'
    GROUP BY student_id
) query_in
GROUP BY rate_group
ORDER BY 1;
```


```
WITH table1 (step_name, correct, count) AS (   
SELECT 
  step_name, 
  SUM( IF (result = 'correct' , 1 , 0)) AS s, 
  COUNT(result) AS c
  FROM step 
  JOIN step_student USING (step_id)
  GROUP BY step_name
    )

SELECT  step_name AS Шаг, ROUND((correct/count)*100) AS Успешность
FROM table1
ORDER BY 2, 1
```
```
WITH get_passed (student_name, pssd)
    AS
        (
           SELECT student_name, COUNT(DISTINCT step_id) AS passed
           FROM student JOIN step_student USING(student_id)
           WHERE result = "correct"
           GROUP BY student_id
           ORDER BY passed
         )
SELECT student_name AS Студент, ROUND(100*pssd/(SELECT COUNT(DISTINCT step_id) FROM step_student)) AS Прогресс,
    CASE
        WHEN ROUND(100*pssd/(SELECT COUNT(DISTINCT step_id) FROM step_student)) =  100 THEN "Сертификат с отличием"
        WHEN ROUND(100*pssd/(SELECT COUNT(DISTINCT step_id) FROM step_student)) >= 80 THEN "Сертификат"
        ELSE ""
    END AS Результат
FROM get_passed
ORDER BY Прогресс DESC, Студент
```

```
SELECT student_name AS Студент, 
    CONCAT(LEFT(step_name, 20), '...') AS Шаг, 
    result AS Результат, 
    FROM_UNIXTIME(submission_time) AS Дата_отправки,
    SEC_TO_TIME(submission_time - LAG(submission_time, 1, submission_time) OVER (ORDER BY submission_time)) AS Разница
FROM student
    INNER JOIN step_student USING(student_id)
    INNER JOIN step USING(step_id)
WHERE student_name = 'student_61'
ORDER BY Дата_отправки;
```

```
SELECT ROW_NUMBER() OVER (ORDER BY Среднее_время) AS Номер,
    Урок, Среднее_время
FROM(
    SELECT 
        Урок, ROUND(AVG(difference), 2) AS Среднее_время
FROM
     (SELECT student_id,
             CONCAT(module_id, '.', lesson_position, ' ', lesson_name) AS Урок,
             SUM((submission_time - attempt_time) / 3600) AS difference
      FROM module INNER JOIN lesson USING (module_id)
                  INNER JOIN step USING (lesson_id)
                  INNER JOIN step_student USING (step_id)
      WHERE submission_time - attempt_time <= 4 * 3600
      GROUP BY 1, 2) AS query_1
GROUP BY 1) AS TA
order by 3;
```



```
SELECT  module_id AS Модуль, student_name AS Студент, COUNT(DISTINCT step_id) AS Пройдено_шагов ,
	ROUND(COUNT(DISTINCT step_id) / 
      MAX(COUNT(DISTINCT step_id)) OVER(PARTITION BY module_id) *100, 1) AS Относительный_рейтинг
FROM lesson 
	JOIN step USING (lesson_id)
	JOIN step_student USING (step_id)
	JOIN student USING (student_id)
WHERE result = 'correct'
GROUP BY module_id, student_name
ORDER BY 1, 4 DESC, 2
```


```
WITH get_time_lesson(student_name,  lesson, max_submission_time)
AS(
    SELECT student_name,  CONCAT(module_id, '.', lesson_position), MAX(submission_time)
    FROM step_student INNER JOIN step USING (step_id)
                          INNER JOIN lesson USING (lesson_id)
                          INNER JOIN student USING(student_id)
    WHERE  result = 'correct'  
    GROUP BY 1,2
    ORDER BY 1),
get_students(student_name)
AS(
    SELECT student_name 
    FROM get_time_lesson
    GROUP BY student_name
    HAVING COUNT(lesson) = 3)
SELECT student_name as Студент,  
       lesson as Урок, 
       FROM_UNIXTIME(max_submission_time) as Макс_время_отправки, 
       IFNULL(CEIL((max_submission_time - LAG(max_submission_time) OVER (PARTITION BY student_name ORDER BY max_submission_time )) / 86400),'-') as Интервал 
FROM get_time_lesson
WHERE student_name in (SELECT * FROM get_students)
ORDER BY 1,3;
```

```
SET @avg_time := (SELECT CEIL(AVG(submission_time - attempt_time))
FROM step_student INNER JOIN student USING(student_id)
WHERE student_name = "student_59" AND (submission_time - attempt_time) < 3600);
WITH get_stat
AS
(
SELECT student_name, CONCAT(module_id, ".", lesson_position, ".", step_position) AS less, step_id, RANK() OVER (PARTITION BY CONCAT(module_id, ".", lesson_position, ".", step_position) ORDER BY submission_time) AS rang, result, 
CASE
    WHEN (submission_time - attempt_time) > 3600 THEN @avg_time
    ELSE (submission_time - attempt_time)
END AS qr
FROM student 
    INNER JOIN step_student USING(student_id)
    INNER JOIN step USING(step_id)
    INNER JOIN lesson USING(lesson_id)
WHERE student_name = "student_59"
)
SELECT student_name AS Студент, less AS Шаг, rang AS Номер_попытки, result AS Результат, SEC_TO_TIME(CEIL(qr)) AS Время_попытки, ROUND((qr / (SUM(qr) OVER (PARTITION BY less ORDER BY less)) * 100), 2) AS Относительное_время
FROM get_stat
ORDER BY step_id, 3;
```

```
WITH qr
AS
(
SELECT student_name, step_id, count(result) AS sm
FROM step_student INNER JOIN student USING(student_id)
WHERE result = "correct"
GROUP BY 1, 2
HAVING count(result) > 1
)
SELECT "I" AS Группа, student_name AS Студент, count(step_id) AS Количество_шагов
FROM (
SELECT student_name, step_id, IF(result = "correct" AND submission_time < MAX(submission_time) OVER (PARTITION BY student_name, step_id), IF(LEAD(result) OVER (PARTITION BY student_id, step_id ORDER BY submission_time) = "wrong", 1, 0), 0) AS change_res
FROM step_student INNER JOIN student USING(student_id))qr1
WHERE change_res = 1
GROUP BY 1, 2
UNION 
SELECT "II" AS Группа, student_name AS Студент, count(step_id) AS Количество_шагов
FROM qr
GROUP BY 1, 2
UNION
SELECT "III" AS Группа, student_name AS Студент, count(DISTINCT step_id) AS Количество_шагов
FROM (
    SELECT student_id, step_id, SUM(new_result) OVER (PARTITION BY student_id, step_id) AS sum_result
    FROM (
        SELECT student_id, step_id, IF(result = "wrong", 0, 1) AS new_result 
            FROM step_student)qr_1)qr_2 INNER JOIN student USING(student_id)
WHERE sum_result = 0
GROUP BY student_id
ORDER BY Группа, Количество_шагов DESC, Студент;
```