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































