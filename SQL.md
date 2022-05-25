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

#### Выбор уникальных элементов столбца. Отобрать различные (уникальные) элементы столбца amount таблицы book.

```
SELECT amount
FROM book
GROUP BY amount;
```
#### Посчитать, количество различных книг и количество экземпляров книг каждого автора , хранящихся на складе.  Столбцы назвать Автор, Различных_книг и Количество_экземпляров соответственно.

```
SELECT author as Автор, COUNT(title) as Различных_книг, SUM(amount) as Количество_экземпляров
FROM book
GROUP BY author;
```

#### Вывести фамилию автора, минимальную, максимальную и среднюю цену книг каждого автора . Вычисляемые столбцы назвать Минимальная_цена, Максимальная_цена и Средняя_цена соответственно.

```
SELECT author,
    MIN(price) AS "Минимальная_цена", 
    MAX(price) AS "Максимальная_цена",
    AVG(price) AS "Средняя_цена"
FROM book
GROUP BY author;
```

####     Для каждого автора вычислить суммарную стоимость книг S (имя столбца Стоимость), а также вычислить налог на добавленную стоимость  для полученных сумм (имя столбца НДС ) , который включен в стоимость и составляет k = 18%,  а также стоимость книг  (Стоимость_без_НДС) без него. Значения округлить до двух знаков после запятой. В запросе для расчета НДС(tax)  и Стоимости без НДС(S_without_tax) использовать следующие формулы:
```
SELECT author, 
    SUM(price*amount) AS Стоимость, 
    ROUND(SUM(price*amount)*0.18 / 1.18, 2) AS НДС,
    ROUND(SUM(price*amount) / 1.18,2) AS Стоимость_без_НДС
FROM book
GROUP BY author;
```

#### Вывести  цену самой дешевой книги, цену самой дорогой и среднюю цену книг на складе. Названия столбцов Минимальная_цена, Максимальная_цена, Средняя_цена соответственно. Среднюю цену округлить до двух знаков после запятой.

```
SELECT MIN(price) AS "Минимальная_цена", 
    MAX(price) AS "Максимальная_цена", 
    ROUND(AVG(price),2) AS 'Средняя_цена'
FROM book;
```

#### Вычислить среднюю цену и суммарную стоимость тех книг, количество экземпляров которых принадлежит интервалу от 5 до 14, включительно. Столбцы назвать Средняя_цена и Стоимость, значения округлить до 2-х знаков после запятой.

```
SELECT ROUND(AVG(price),2) AS Средняя_цена,
    ROUND(SUM(price * amount),2) AS Стоимость
FROM book
WHERE amount BETWEEN 5 AND 14;
```

#### Посчитать стоимость всех экземпляров каждого автора без учета книг «Идиот» и «Белая гвардия». В результат включить только тех авторов, у которых суммарная стоимость книг более 5000 руб. Вычисляемый столбец назвать Стоимость. Результат отсортировать по убыванию стоимости.

```
SELECT author, 
    SUM(price * amount) as Стоимость 
FROM book 
WHERE title <> 'Идиот' and title <> 'Белая гвардия' 
GROUP BY author 
HAVING SUM(price*amount) > 5000 
ORDER BY Стоимость DESC 
```

#### Придумайте один или несколько запросов к нашей таблице book, используя групповые функции. Проверьте, правильно ли они работают.

```
SELECT book_id AS Порядковыйномер, author AS Автор, ROUND(AVG(price)) AS Средняяцена
FROM book
GROUP BY book_id, author
HAVING Порядковый_номер > 2
```



