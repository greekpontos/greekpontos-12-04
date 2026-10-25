# Домашнее задание к занятию «SQL. Часть 2» — Петров Виктор

### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия.
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md.
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`).
   5. В личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.

---

### Задание 1
Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

```sql
SELECT 
    staff.first_name, 
    staff.last_name, 
    city.city, 
    COUNT(c.customer_id) AS customers_count
FROM store s
JOIN staff ON s.manager_staff_id = staff.staff_id
JOIN address a ON s.address_id = a.address_id
JOIN city ON a.city_id = city.city_id
JOIN customer c ON s.store_id = c.store_id
GROUP BY s.store_id, staff.staff_id, city.city_id
HAVING customers_count > 300;
```

---

### Задание 2
Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

```sql
SELECT COUNT(*) AS films_count
FROM film
WHERE length > (SELECT AVG(length) FROM film);
```

---

### Задание 3
Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

```sql
SELECT 
    DATE_FORMAT(payment_date, '%Y-%m') AS payment_month, 
    SUM(amount) AS total_amount, 
    COUNT(rental_id) AS total_rentals
FROM payment
GROUP BY payment_month
ORDER BY total_amount DESC
LIMIT 1;
```

---

### Задание 4* (Дополнительное)
Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

```sql
SELECT 
    staff_id, 
    COUNT(payment_id) AS sales_count,
    CASE WHEN COUNT(payment_id) > 8000 THEN 'Да' ELSE 'Нет' END AS Премия
FROM payment
GROUP BY staff_id;
```
