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

INSERT INTO buy_book (buy_id, book_id, amount)
VALUES
    (5, (SELECT book_id FROM 
         book JOIN author USING(author_id) 
         WHERE title='Лирика' AND name_author LIKE 'Пастернак%'), 2),
    (5, (SELECT book_id 
         FROM book JOIN author USING(author_id) 
         WHERE title='Белая Гвардия' AND name_author LIKE 'Булгаков%'), 1);
SELECT * FROM buy_book;








































