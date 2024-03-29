# Домашнее задание к занятию «SQL. Часть 2» Константин Чумаков.

### Инструкция по выполнению домашнего задания

1. Сделайте fork [репозитория c шаблоном решения](https://github.com/netology-code/sys-pattern-homework) к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/gitlab-hw или https://github.com/имя-вашего-репозитория/8-03-hw).
2. Выполните клонирование этого репозитория к себе на ПК с помощью команды `git clone`.
3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
   - впишите вверху название занятия и ваши фамилию и имя;
   - в каждом задании добавьте решение в требуемом виде: текст/код/скриншоты/ссылка;
   - для корректного добавления скриншотов воспользуйтесь инструкцией [«Как вставить скриншот в шаблон с решением»](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md);
   - при оформлении используйте возможности языка разметки md. Коротко об этом можно посмотреть в [инструкции по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md).
4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`).
5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
6. Любые вопросы задавайте в чате учебной группы и/или в разделе «Вопросы по заданию» в личном кабинете.

Желаем успехов в выполнении домашнего задания.

---

Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.   

### Решение 1   
```
SELECT staff.first_name, staff.last_name, city.city, COUNT(customer.customer_id) as total_customers
    FROM staff
    JOIN store ON staff.store_id = store.store_id
    JOIN address ON store.address_id = address.address_id
    JOIN city ON address.city_id = city.city_id
    JOIN customer ON store.store_id = customer.store_id
    GROUP BY staff.first_name, staff.last_name, city.city
    HAVING COUNT(customer.customer_id) > 300;

```
![alt text](https://github.com/BudyGun/sql-2/blob/main/images/sq1.png)

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.   

### Решение 2   
```
SELECT COUNT(*) FROM film WHERE length > (SELECT AVG(length) FROM film);
```
![alt text](https://github.com/BudyGun/sql-2/blob/main/images/sq2.png)


### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.   

### Решение 3   
```
SELECT YEAR(payment_date) AS year, MONTH(payment_date) AS month,
COUNT(DISTINCT rental.rental_id) as rental_count, SUM(amount) as total_payment
FROM payment
JOIN rental ON payment.rental_id = rental.rental_id
GROUP BY year, month
ORDER BY total_payment DESC
LIMIT 1;
```
![alt text](https://github.com/BudyGun/sql-2/blob/main/images/sq3.png)

## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 4*

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

### Задание 5*

Найдите фильмы, которые ни разу не брали в аренду.
