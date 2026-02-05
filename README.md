# Домашнее задание к занятию «Расширенные возможности SQL» - Кузнецов Евгений Александрович

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

### Решение 1

Скриншот 1 к заданию 1  
![Screen 1](https://github.com/jack34ru/SQL-2/blob/main/screenshots/Screenshot_163.png)

Листинг  кода:  
```sql
SELECT
  CONCAT(s.first_name, ' ', s.last_name) AS staff_name,
  c.city AS city,
  COUNT(cu.customer_id) AS customer_count
FROM store st
JOIN staff s     ON s.store_id = st.store_id
JOIN address a   ON a.address_id = st.address_id
JOIN city c      ON c.city_id = a.city_id
JOIN customer cu ON cu.store_id = st.store_id
GROUP BY st.store_id, s.staff_id, c.city
HAVING COUNT(cu.customer_id) > 300;
```

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

### Решение 2

Скриншот 1 к заданию 2  
![Screen 1](https://github.com/jack34ru/SQL-2/blob/main/screenshots/Screenshot_164.png)

Листинг  кода:  
```sql
SELECT COUNT(*) AS film_count
FROM film
WHERE length > (SELECT AVG(length) FROM film);
```

### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

### Решение 3

Скриншот 1 к заданию 3  
![Screen 1](https://github.com/jack34ru/SQL-2/blob/main/screenshots/Screenshot_165.png)

Листинг  кода:  
```sql
SELECT
  DATE_FORMAT(p.payment_date, '%Y-%m') AS month,
  SUM(p.amount) AS total_amount,
  COUNT(r.rental_id) AS rental_count
FROM payment p
JOIN rental r ON r.rental_id = p.rental_id
GROUP BY month
ORDER BY total_amount DESC
LIMIT 1;
```