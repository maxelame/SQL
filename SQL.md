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

#### 