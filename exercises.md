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

# 10

Найдите модели принтеров, имеющих самую высокую цену. Вывести: model, price

```sql
select model, price
from printer
where 
price = (select max(price) as max_price from printer)

```

# 11

Найдите среднюю скорость ПК.

```sql
select avg(speed)
from pc
```

# 12

Найдите среднюю скорость ПК-блокнотов, цена которых превышает 1000 дол.


```sql
select avg(speed) as avg_speed
from laptop
where price>1000
```

# 13

Найдите среднюю скорость ПК, выпущенных производителем A.

```sql
SELECT AVG(SPEED) AS avg_speed
FROM pc
WHERE model IN (SELECT model FROM product WHERE maker='a')
```

# 14

Найдите класс, имя и страну для кораблей из таблицы Ships, имеющих не менее 10 орудий.

```sql
select classes.class as class, name, country
from classes join
ships on classes.class = ships.class
where numGuns >= 10
```

# 15

Найдите размеры жестких дисков, совпадающих у двух и более PC. Вывести: HD

```sql
select hd
from pc
group by hd
having count(hd)>=2
```

# 16

Найдите пары моделей PC, имеющих одинаковые скорость и RAM. В результате каждая пара указывается только один раз, т.е. (i,j), но не (j,i), Порядок вывода: модель с большим номером, модель с меньшим номером, скорость и RAM.

```sql
SELECT distinct pc1.model, pc2.model, pc1.speed, pc1.ram
FROM pc AS pc1, pc AS pc2
WHERE pc1.model>pc2.model AND pc1.speed=pc2.speed AND pc1.ram=pc2.ram
```

# 17

Найдите модели ПК-блокнотов, скорость которых меньше скорости каждого из ПК.
Вывести: type, model, speed

```sql
SELECT DISTINCT p.type, l.model, l.speed
FROM Product p, Laptop l
WHERE l.speed < (SELECT MIN(speed) FROM PC) AND p.type = 'laptop'
```

# 18

Найдите производителей самых дешевых цветных принтеров. Вывести: maker, price

```sql
WITH t1 AS (
SELECT model, price
FROM printer
WHERE color = 'y')

SELECT DISTINCT maker, price
FROM t1
JOIN product p ON t1.model = p.model
WHERE price = (SELECT MIN(price) FROM t1)
```

# 19

Для каждого производителя, имеющего модели в таблице Laptop, найдите средний размер экрана выпускаемых им ПК-блокнотов.
Вывести: maker, средний размер экрана.

```sql
SELECT maker, AVG(screen) AS avg_screen
FROM product 
JOIN laptop ON laptop.model = product.model
GROUP BY maker
```

# 20

Найдите производителей, выпускающих по меньшей мере три различных модели ПК. Вывести: Maker, число моделей ПК.

```sql
SELECT maker, COUNT(model) AS count_model
FROM product
WHERE type = 'pc'
GROUP BY maker
HAVING COUNT(model) >= 3
```