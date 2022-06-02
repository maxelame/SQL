# Stepik SQL
```
SELECT buy_book.buy_id, title, price, buy_book.amount
FROM 
    client 
    INNER JOIN buy ON client.client_id = buy.client_id
    INNER JOIN buy_book ON buy_book.buy_id = buy.buy_id
    INNER JOIN book ON buy_book.book_id=book.book_id
WHERE name_client LIKE 'Баранов Павел'
ORDER BY buy_book.buy_id, title;
```
```
SELECT author.name_author, book.title, COUNT(buy_book.book_id) AS Количество
FROM
    book
    INNER JOIN author ON author.author_id = book.author_id
    LEFT JOIN buy_book ON buy_book.book_id = book.book_id
    LEFT JOIN buy ON buy.buy_id = buy_book.buy_id
GROUP BY author.name_author, book.title
ORDER BY author.name_author, book.title
```

```
SELECT city.name_city, COUNT(client_id) AS Количество
FROM buy
    INNER JOIN client USING (client_id)
    INNER JOIN city USING (city_id)
GROUP BY buy.client_id
ORDER BY Количество DESC, name_city;
```
```
SELECT buy_id, date_step_end 
FROM step
    INNER JOIN buy_step ON step.step_id = buy_step.step_id
WHERE buy_step.step_id = 1 and date_step_end IS NOT Null;
```
```
SELECT buy_id, name_client, SUM(price * buy_book.amount) as Стоимость
FROM buy
    INNER JOIN client using(client_id)
    INNER JOIN buy_book using(buy_id)
    INNER JOIN book using(book_id)
GROUP BY buy_book.buy_id
ORDER BY 1;
```

```
SELECT buy_id, name_step
FROM buy_step
     JOIN step USING(step_id)
WHERE date_step_beg IS NOT NULL and date_step_end IS NULL
ORDER BY buy_id
```






















