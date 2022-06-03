# Stepik SQL

```
INSERT INTO client(name_client,city_id,email)
SELECT 'Попов Илья', city_id, 'popov@test'
FROM city
WHERE name_city = 'Москва';
SELECT * FROM client
```



```
INSERT buy (buy_description, client_id)
SELECT 'Связаться со мной по вопросу доставки', client_id
FROM client
WHERE name_client IN ('Попов Илья');
```
```
INSERT INTO buy_book (buy_id, book_id, amount)
VALUES
    (5, (SELECT book_id FROM 
         book JOIN author USING(author_id) 
         WHERE title='Лирика' AND name_author LIKE 'Пастернак%'), 2),
    (5, (SELECT book_id 
         FROM book JOIN author USING(author_id) 
         WHERE title='Белая Гвардия' AND name_author LIKE 'Булгаков%'), 1);
SELECT * FROM buy_book;
```

```
UPDATE book, buy_book
SET    book.amount = book.amount - buy_book.amount
WHERE  buy_book.buy_id = 5 AND book.book_id = buy_book.book_id;

SELECT * FROM book
```

```
CREATE TABLE buy_pay AS
SELECT 
    title,
    name_author,
    book.price,
    buy_book.amount,
    book.price * buy_book.amount AS 'Стоимость'
FROM
    buy_book
    INNER JOIN book USING (book_id)
    INNER JOIN author USING (author_id)
WHERE
    buy_id = 5
ORDER BY title
```


```
CREATE TABLE buy_pay
SELECT buy_id, sum(buy_book.amount) as Количество, sum(book.price*buy_book.amount) as Итого
FROM buy_book
JOIN book USING(book_id)
JOIN author USING(author_id)
WHERE buy_id=5
GROUP BY 1;

SELECT * FROM buy_pay;
```


```
INSERT INTO buy_step (buy_id, step_id, date_step_beg, date_step_end)
SELECT buy_id, step_id, Null, Null
FROM buy
CROSS JOIN step
WHERE buy_id = 5;
```





























