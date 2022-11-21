# 1

Найдите номер модели, скорость и размер жесткого диска для всех ПК стоимостью менее 500 дол. Вывести: model, speed и hd.

```sql
select model, speed, hd
from pc
where price <500
```

# 2

Найдите производителей принтеров. Вывести: maker.

```sql
select distinct maker
from product
where type='printer'
```

# 3

Найдите номер модели, объем памяти и размеры экранов ПК-блокнотов, цена которых превышает 1000 дол.

```sql
select model, ram, screen
from laptop
where price >1000
```

# 4

Найдите все записи таблицы Printer для цветных принтеров.

```sql
select code, model, color, type, price
from printer
where color='y'
```

# 5

Найдите номер модели, скорость и размер жесткого диска ПК, имеющих 12x или 24x CD и цену менее 600 дол.

```sql
select model, speed, hd
from pc
where price<600 and (cd='12x' or cd='24x')
```

# 6

Для каждого производителя, выпускающего ПК-блокноты c объёмом жесткого диска не менее 10 Гбайт, найти скорости таких ПК-блокнотов. 
Вывод: производитель, скорость.

```sql
select distinct maker, speed
from laptop inner join product on product.model=laptop.model
where hd>=10
```

# 7

Найдите номера моделей и цены всех имеющихся в продаже продуктов (любого типа) производителя B (латинская буква).

```sql
SELECT a.model, price
from (select model, price 
FROM pc 
UNION
SELECT model, price
FROM Laptop 
UNION
SELECT model, price
FROM Printer 
) as a join
product p on a.model=p.model
where p.maker='b'
```

# 8

Найдите производителя, выпускающего ПК, но не ПК-блокноты.

```sql
select maker 
from product 
where type='pc'
except
select maker 
from product
where type='laptop'
```

# 9

Найдите производителей ПК с процессором не менее 450 Мгц. Вывести: Maker

```sql
select distinct maker
from product join pc on pc.model=product.model
where speed>=450 and type='pc'
```

#10

Найдите модели принтеров, имеющих самую высокую цену. Вывести: model, price

```sql
select model, price
from printer
where 
price = (select max(price) as max_price from printer)

```
