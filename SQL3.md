# Stepik SQL

```
INSERT INTO client(name_client,city_id,email)
SELECT 'Попов Илья', city_id, 'popov@test'
FROM city
WHERE name_city = 'Москва';
SELECT * FROM client
```




INSERT buy (buy_description, client_id)
SELECT 'Связаться со мной по вопросу доставки', client_id
FROM client
WHERE name_client IN ('Попов Илья');










































