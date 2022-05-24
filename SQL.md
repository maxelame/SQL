# Stepik SQL


#### Создание таблицы
```
CREATE TABLE book(
    book_id INT PRIMARY KEY AUTO_INCREMENT, 
    title VARCHAR(50), 
    author VARCHAR(30), 
    price DECIMAL(8, 2), 
    amount INT
);
```

#### Вставка записи в таблицу

```
INSERT INTO book (title, author, price, amount) 
VALUES ('Мастер и Маргарита', 'Булгаков М.А.', 670.99, 3 );
```

#### Добавление записей

```
INSERT INTO book (title, author, price, amount) 
VALUES ('Белая гвардия', 'Булгаков М.А.', 540.50 , 5 ),
('Идиот', 'Достоевский Ф.М.', 460.00, 10 ),
('Братья Карамазовы', 'Достоевский Ф.М.', 799.01, 2 );
```

#### Выборка всех данных из таблицы

```
select * from book /* выборка всех данных таблицы */
```


#### Выборка отдельных столбцов

```
select author, title, price 
from book
```


#### Выборка новых столбцов и присвоение им новых имен

```
select title Название, author Автор 
from book
```

#### Выборка данных с созданием вычисляемого столбца

```
select title, amount, amount * 1.65 as pack from book
```

#### Выборка данных, вычисляемые столбцы, математические функции

```
select title, author, amount, ROUND(price*0.7,2) as new_price 
from book
```

#### Выборка данных, вычисляемые столбцы, логические функции

```
select author, title, ROUND(if(author = "Булгаков М.А.", price*1.1, if(author = "Есенин С.А.", price*1.05, price*1)),2) as new_price 
from book
```

#### Выборка данных по условию
```
select author, title, price 
from book 
where amount < 10
```

#### Выборка данных, логические операции

```
select title, author, price, amount 
from book 
where (price < 500 or price > 600) and price * amount >= 5000
```

#### Выборка данных, операторы BETWEEN, IN
```
select title, author
from book
where price between 540.50 and 800 and amount in (2,3,5,7)
```

#### Выборка данных с сортировкой

```
select author, title 
from book
where amount between 2 and 14
order by 1 desc, 2 
```

#### Выборка данных, оператор LIKE

```
select title, author from book
where (title like "_% _%") and author like "%С.%"
order by title
```