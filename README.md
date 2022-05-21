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

