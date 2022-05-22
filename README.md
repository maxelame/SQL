# SQL

#### Создаем таблицу
```
CREATE TABLE months (id int, name varchar(10), days int);
```

#### Ввод данных
```
INSERT INTO months VALUES (1,'January',31);
INSERT INTO months (id,name,days) VALUES (2,'February',29);
```
#### Select
```
SELECT name, weapon FROM characters
SELECT name, weapon FROM characters ORDER BY name DESC
```
#### Where
```
SELECT * 
FROM characters
WHERE weapon = 'pistol';
```
#### И/или
```
SELECT * 
FROM albums 
WHERE genre = 'rock' AND sales_in_millions <= 50 
ORDER BY released
```
#### In/Between/Like
##### Условия в WHERE могут быть записаны с использованием ещё нескольких команд, которыми являются:

    IN - сравнивает значение в столбце с несколькими возможными значениями и возвращает true, если значение совпадает хотя бы с одним значением
    BETWEEN - проверяет, находится ли значение в каком-то промежутке
    LIKE - ищет по шаблону

К примеру, мы можем сделать запрос для вывода данных об альбомах в жанре pop или soul:
```
SELECT * FROM albums WHERE genre IN ('pop','soul');
```
 Если мы хотим вывести все альбомы, которые были выпущены в промежутке между 1975 и 1985 годом, мы можем использовать следующую запись:
```
SELECT * FROM albums WHERE released BETWEEN 1975 AND 1985;
```

Также, если мы хотим вывести все альбомы, в названии которых есть буква 'R', мы можем использовать следующую запись:
```
SELECT * FROM albums WHERE album LIKE '%R%';
```
Знак % означает любую последовательность символов (0 символов тоже считается за последовательность).

Если мы хотим вывести все альбомы, первая буква в названии которых - 'R', то запись слегка изменится:
```
SELECT * FROM albums WHERE album LIKE 'R%';
```
В SQL также есть инверсия. Для примера, попробуйте самостоятельно написать NOT перед любым логическим выражением в условии (NOT BETWEEN и так далее).


#### Функции

В SQL полно встроенных функций для выполнения разных операций. Мы же покажем вам только наиболее часто используемые:

    COUNT() - возвращает число строк
    SUM() - возвращает сумму всех полей с числовыми значениями в них
    AVG() - возвращает среднее значение среди строк
    MIN()/MAX() - возвращает минимальное/максимальное значение среди строк

Чтобы вывести год выпуска самого старого альбома, в таблице можно использовать следующий запрос:
```
SELECT MIN(released) FROM albums;
```
Обратите внимание, что если вы напишете запрос, в котором вам, к примеру, нужно будет вывести имя и среднее значение чего-либо, то вы получите ошибку на выводе.

Допустим, вы пишете такой запрос:
```
SELECT name, avg(age) FROM students;
```
Чтобы избежать ошибки, вам следует добавить следующую строку:
```
GROUP BY name
```
Причиной тому является, что запись avg(age) является совокупной (aggregated), и вам необходимо группировать значения по имени.