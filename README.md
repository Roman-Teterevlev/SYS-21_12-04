Roman Teterevlev

SQL. Часть 2

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

### Ответ:
```
select concat(sf.first_name , ' ', sf.last_name) as 'Full Name', cy.city, COUNT(cr.customer_id) as 'Count Consumers'		
from sakila.store s
join sakila.staff sf on sf.store_id = s.store_id 
join sakila.customer cr on cr.store_id = s.store_id
join sakila.address a on a.address_id = s.address_id 
join sakila.city cy on cy.city_id = a.city_id 
group by sf.staff_id, cy.city_id 
having COUNT(cr.customer_id) > 300;
```
![alt text](https://github.com/Roman-Teterevlev/SYS-21_12-04/blob/main/12-04_1.png)

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

### Ответ:
```
select COUNT(f.title)
from sakila.film f  
where f.`length` > (select AVG(`length`) from sakila.film);
```
![alt text](https://github.com/Roman-Teterevlev/SYS-21_12-04/blob/main/12-04_2.png)

### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

### Ответ:
```
select t.amount_of_payments, t.month_of_payments, (select count(r.rental_id) from sakila.rental r where DATE_FORMAT(r.rental_date, '%M %Y') = t.month_of_payments) 'count_of_rent'
from (select SUM(p.amount) 'amount_of_payments', DATE_FORMAT(p.payment_date, '%M %Y') 'month_of_payments' from sakila.payment p group by DATE_FORMAT(p.payment_date, '%M %Y')) t
order by t.amount_of_payments desc  
limit 1;
```
![alt text](https://github.com/Roman-Teterevlev/SYS-21_12-04/blob/main/12-04_3.png)

