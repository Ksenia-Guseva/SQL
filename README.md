Задачи, SQL-запросы:

1.Посчитай, сколько пользователей зарегистрировано в системе. Это таблица user_model. В результате выведи только количество пользователей

SELECT COUNT (Id) FROM user_model;

2.Добавь три новых разных продукта в таблицу product_model.

INSERT INTO
product_model (name, price, weight, units, "categoryId")
VALUES ('Чай Бабушкин сад',239, 500,'г' , 1),
('Батончик Степ',43, 50,'г' , 1),
('Сметана Домик в деревне',100, 150,'г' , 1);

3.Посчитай количество продуктов в каждой категории и вывести id только тех категорий, в которых количество продуктов больше пяти. Это таблица product_model. Результат отсортируй в порядке возрастания количества продуктов.

SELECT "categoryId", COUNT(*) FROM product_model
GROUP BY "categoryId"
HAVING COUNT(*) > 5
ORDER BY COUNT(*);

4.В приложение хотят добавить фичу — возможность вносить правки в заказы. Сработает только с теми заказами, где:
стоимость доставки (deliveryPrice) больше 500,
стоит статус «заказ формируется» или «заказ в доставке». Напиши запрос, который будет выводить в системе id всех заказов и возможность внести правки. Назови эту колонку update_order. Если статус заказа позволяет вносить изменения, то в колонку update_order нужно вывести yes. Если правки внести нельзя — вывести no.

SELECT id,
(CASE WHEN ("deliveryPrice" > 500
AND status IN (0, 1))
THEN 'yes'
ELSE 'no'
END) AS update_order
FROM order_model;

5.Выведи информацию о продуктах, цена которых находится в диапазоне от 200 до 500. Информация по каждому продукту включает: название продукта, цену, название категории, к которой он относится.

SELECT p.name, p.price, c.name
FROM product_model AS p
INNER JOIN category_model AS c ON p."categoryId" = c.id
WHERE p.price BETWEEN 200 AND 500;

6.Для каждой карточки выведи ее название и количество продуктов (productsCount) для этой карточки. Результат отсортируй по названию карточки.

SELECT c.name, SUM(k."productsCount")
FROM card_model AS c LEFT OUTER JOIN kit_model AS k ON c.id=k."cardId"
GROUP BY c.name
ORDER BY c.name;
 
