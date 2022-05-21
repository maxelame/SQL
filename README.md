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

