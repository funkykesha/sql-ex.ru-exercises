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

# 21

Найдите максимальную цену ПК, выпускаемых каждым производителем, у которого есть модели в таблице PC.
Вывести: maker, максимальная цена.

```sql
SELECT maker, MAX(price) as max_price
FROM product 
JOIN pc ON pc.model = product.model
GROUP BY maker
```

# 22

Для каждого значения скорости ПК, превышающего 600 МГц, определите среднюю цену ПК с такой же скоростью. Вывести: speed, средняя цена.

```sql
SELECT speed, AVG(price) as avg_price
FROM pc
WHERE speed > 600
GROUP BY speed
```

# 23

Найдите производителей, которые производили бы как ПК
со скоростью не менее 750 МГц, так и ПК-блокноты со скоростью не менее 750 МГц.
Вывести: Maker

```sql
SELECT maker
FROM product 
JOIN pc ON PC.model = product.model
WHERE speed >= 750
GROUP BY maker
INTERSECT
SELECT maker
FROM product 
JOIN laptop ON laptop.model = product.model
WHERE speed >= 750
GROUP BY maker
```

# 24

Перечислите номера моделей любых типов, имеющих самую высокую цену по всей имеющейся в базе данных продукции.


```sql
WITH all_model AS (
	SELECT model, MAX(price) AS price FROM pc GROUP BY model
	UNION ALL
	SELECT model, MAX(price) AS price FROM laptop GROUP BY model
	UNION ALL
	SELECT model, MAX(price) as price FROM printer GROUP BY model
	)

SELECT model 
FROM all_model 
WHERE price = (
	SELECT MAX(price) FROM all_model
	)
```

# 25

Найдите производителей принтеров, которые производят ПК с наименьшим объемом RAM и с самым быстрым процессором среди всех ПК, имеющих наименьший объем RAM. Вывести: Maker


```sql
WITH pc_maker AS (
    SELECT maker
    FROM product
    WHERE model in (
	    SELECT model
	    FROM pc
	    WHERE speed IN (
	        SELECT MAX(speed) FROM pc WHERE ram = (
	            SELECT MIN(ram) FROM pc
	            )
            ) AND ram IN (
            SELECT MIN(ram) FROM pc
            )
	    )
	)
SELECT DISTINCT product.maker
FROM product
JOIN pc_maker ON product.maker = pc_maker.maker
WHERE type = 'printer'
```


# 26

Найдите среднюю цену ПК и ПК-блокнотов, выпущенных производителем A (латинская буква). Вывести: одна общая средняя цена.

```sql
WITH speed_maker_product AS (
	SELECT price
	FROM pc
	WHERE model IN (
	    SELECT model
	    FROM product
	    WHERE maker = 'A'
	)
	UNION ALL
	SELECT price
	FROM laptop
	WHERE model IN (
	    SELECT model
	    FROM product
	    WHERE maker = 'A'
	)
)

SELECT AVG(price)
FROM speed_maker_product
```

# 27

Найдите средний размер диска ПК каждого из тех производителей, которые выпускают и принтеры. Вывести: maker, средний размер HD.

```sql
SELECT maker, AVG(hd)
FROM product
JOIN pc ON product.model = pc.model
WHERE maker IN (
    SELECT maker
    FROM product
    WHERE type = 'printer'
)
GROUP BY maker
```

# 28

Используя таблицу Product, определить количество производителей, выпускающих по одной модели.

```sql
SELECT COUNT(maker) AS qty_maker
FROM (
	SELECT maker
	FROM product
	GROUP BY maker
	HAVING COUNT(model) = 1
	) AS qty
```

# 29

В предположении, что приход и расход денег на каждом пункте приема фиксируется не чаще одного раза в день [т.е. первичный ключ (пункт, дата)], написать запрос с выходными данными (пункт, дата, приход, расход). Использовать таблицы Income_o и Outcome_o.


```sql
SELECT Income_o.point, Income_o.date, inc, out
FROM Income_o
LEFT JOIN outcome_o ON Income_o.point = outcome_o.point AND Income_o.date = outcome_o.date
UNION
SELECT outcome_o.point, outcome_o.date, inc, out
FROM outcome_o
LEFT JOIN income_o ON Income_o.point = outcome_o.point AND Income_o.date = outcome_o.date
```

# 30

В предположении, что приход и расход денег на каждом пункте приема фиксируется произвольное число раз (первичным ключом в таблицах является столбец code), требуется получить таблицу, в которой каждому пункту за каждую дату выполнения операций будет соответствовать одна строка.
Вывод: point, date, суммарный расход пункта за день (out), суммарный приход пункта за день (inc). Отсутствующие значения считать неопределенными (NULL).

```sql
WITH x AS (
	SELECT outcome.point, outcome.date, out, inc
	FROM outcome
	LEFT JOIN income ON income.code = outcome.code AND income.date = outcome.date AND outcome.point = income.point
	UNION ALL
	SELECT income.point, income.date, out, inc
	FROM income
	LEFT JOIN outcome ON income.code = outcome.code AND income.date = outcome.date AND outcome.point = income.point
	)

SELECT point, date, SUM(out), SUM(inc)
FROM x
GROUP BY point, date
```

# 31

Для классов кораблей, калибр орудий которых не менее 16 дюймов, укажите класс и страну.

```sql
SELECT class, country
FROM classes
WHERE bore>=16
```

# 32

Одной из характеристик корабля является половина куба калибра его главных орудий (mw). С точностью до 2 десятичных знаков определите среднее значение mw для кораблей каждой страны, у которой есть корабли в базе данных.

```sql
SELECT country, CAST(avg(bore * bore * bore / 2) AS numeric(6,2)) AS mw
FROM (
	SELECT country, bore, s.name
	FROM classes c 
	JOIN ships s ON c.class = s.class
	UNION
	SELECT country, bore, ship
	FROM classes
	LEFT JOIN outcomes o ON class = ship
	WHERE ship NOT IN (SELECT name FROM ships)
	) a
GROUP BY country
```

# 33

Укажите корабли, потопленные в сражениях в Северной Атлантике (North Atlantic). Вывод: ship.

```sql
SELECT ship
FROM outcomes
WHERE result='sunk' AND battle='north atlantic'
```

# 34

По Вашингтонскому международному договору от начала 1922 г. запрещалось строить линейные корабли водоизмещением более 35 тыс.тонн. Укажите корабли, нарушившие этот договор (учитывать только корабли c известным годом спуска на воду). Вывести названия кораблей.

```sql
SELECT name
FROM ships s
JOIN classes c ON c.class = s.class
WHERE launched >= 1922 AND displacement > 35000 AND type = 'bb'
```

# 35

В таблице Product найти модели, которые состоят только из цифр или только из латинских букв (A-Z, без учета регистра).
Вывод: номер модели, тип модели.

```sql
SELECT model, type
FROM product
WHERE model NOT LIKE '%[^0-9]%' OR model NOT LIKE '%[^a-zA-Z]%'
```

# 36

Перечислите названия головных кораблей, имеющихся в базе данных (учесть корабли в Outcomes).

```sql
SELECT name
FROM classes c
JOIN ships s ON s.class = c.class
WHERE name = c.class
UNION
SELECT ship
FROM classes c
JOIN outcomes o ON o.ship = c.class
```

# 37

Найдите классы, в которые входит только один корабль из базы данных (учесть также корабли в Outcomes).

```sql
WITH x AS (SELECT c.class, s.name
	FROM classes c
	JOIN ships s ON s.class = c.class
	UNION
	SELECT c.class, o.ship
	FROM classes c
	JOIN outcomes o ON o.ship = c.class
	)

SELECT class
FROM x
GROUP BY class
HAVING COUNT(name) = 1
```

# 38

Найдите страны, имевшие когда-либо классы обычных боевых кораблей ('bb') и имевшие когда-либо классы крейсеров ('bc').

```sql
SELECT country
FROM classes
WHERE type = 'bb' 
INTERSECT
SELECT country
FROM classes
WHERE type = 'bc'
```

# 39

Найдите корабли, `сохранившиеся для будущих сражений`; т.е. выведенные из строя в одной битве (damaged), они участвовали в другой, произошедшей позже.

```sql
SELECT DISTINCT o.ship FROM outcomes o
JOIN battles b ON o.battle = b.name
WHERE o.result = 'damaged' 
	AND exists (
		SELECT * FROM outcomes o2
		JOIN battles b2 ON o2.battle = b2.name
		WHERE b2.date > b.date AND o2.ship=o.ship
		)
```

# 40

Найти производителей, которые выпускают более одной модели, при этом все выпускаемые производителем модели являются продуктами одного типа.
Вывести: maker, type

```sql
SELECT maker, type
FROM product
WHERE maker in (
	SELECT maker
	FROM (
		SELECT maker, type
		FROM product
		GROUP BY maker, type
		) x
	GROUP BY maker
	HAVING count(type) = 1
	)
GROUP BY maker, type
HAVING count(model) > 1
```

# 41

Для каждого производителя, у которого присутствуют модели хотя бы в одной из таблиц PC, Laptop или Printer,
определить максимальную цену на его продукцию.
Вывод: имя производителя, если среди цен на продукцию данного производителя присутствует NULL, то выводить для этого производителя NULL,
иначе максимальную цену.


```sql
SELECT maker,
CASE 
WHEN MAX (
	CASE WHEN price IS null then 1 else 0 END
	) = 0
THEN MAX(price)
END m_price
FROM (
	SELECT maker, price FROM pc JOIN product p ON pc.model = p.model
	UNION
	SELECT maker, price FROM laptop JOIN product p ON laptop.model = p.model
	UNION
	SELECT maker, price FROM printer JOIN product p ON printer.model = p.model
	) x
GROUP BY maker
```

# 42

Найдите названия кораблей, потопленных в сражениях, и название сражения, в котором они были потоплены.

```sql
SELECT ship, battle
FROM outcomes
WHERE result = 'sunk'
```

# 43

Укажите сражения, которые произошли в годы, не совпадающие ни с одним из годов спуска кораблей на воду.

```sql
SELECT battles.name battles_name
FROM battles
WHERE YEAR(date) NOT IN (
				SELECT launched FROM ships WHERE launched IS NOT NULL
				)
```

# 44

Найдите названия всех кораблей в базе данных, начинающихся с буквы R.

```sql
SELECT name 
FROM (
	SELECT name
	FROM ships
	UNION
	SELECT ship
	FROM outcomes
	) x
WHERE name LIKE 'R%'
```

# 45

Найдите названия всех кораблей в базе данных, состоящие из трех и более слов (например, King George V).
Считать, что слова в названиях разделяются единичными пробелами, и нет концевых пробелов.

```sql
SELECT name
FROM (
	SELECT name
	FROM ships
	UNION
	SELECT ship
	FROM outcomes
	) x
WHERE name LIKE '% % %'
```

# 46

Для каждого корабля, участвовавшего в сражении при Гвадалканале (Guadalcanal), вывести название, водоизмещение и число орудий.

```sql
SELECT ship, displacement, numGuns
FROM outcomes 
FULL JOIN ships ON ship = name
FULL JOIN classes ON ships.class = classes.class OR outcomes.ship = classes.class
WHERE battle = 'Guadalcanal'
```

# 47

Определить страны, которые потеряли в сражениях все свои корабли.

```sql
WITH t1 AS (
	SELECT country, COUNT(name) as co
	FROM (
		SELECT country, name
		FROM classes
		JOIN ships ON classes.class = ships.class
		UNION
		SELECT country, ship
		FROM classes
		JOIN outcomes ON classes.class = outcomes.ship
		) u1
	GROUP BY country),
t2 as (
	SELECT country, COUNT(name) as co
	FROM (
		SELECT country, name
		FROM classes
		JOIN ships ON classes.class = ships.class
		WHERE name IN (
			SELECT ship 
			FROM outcomes 
			WHERE result = 'sunk'
			)
		UNION
		SELECT country, ship
		FROM classes
		JOIN outcomes ON classes.class = outcomes.ship
		WHERE ship IN (
			SELECT ship 
			FROM outcomes 
			WHERE result = 'sunk'
			)
		) u2
	GROUP BY country)

SELECT t1.country
FROM t1
JOIN t2 ON t1.country = t2.country
WHERE t1.co = t2.co 
	AND t1.country = t2.country
```

# 48

Найдите классы кораблей, в которых хотя бы один корабль был потоплен в сражении.

```sql
SELECT classes.class
FROM classes
JOIN ships ON classes.class = ships.class
WHERE name IN (
	SELECT ship 
	FROM outcomes 
	WHERE result = 'sunk'
	)
UNION
SELECT classes.class
FROM classes
JOIN outcomes ON classes.class = outcomes.ship
WHERE ship IN (
	SELECT ship 
	FROM outcomes 
	WHERE result = 'sunk')
```

# 49

Найдите названия кораблей с орудиями калибра 16 дюймов (учесть корабли из таблицы Outcomes).

```sql
SELECT ships.name
FROM classes
JOIN ships ON classes.class = ships.class
WHERE bore = 16
UNION
SELECT outcomes.ship
FROM classes
JOIN outcomes ON classes.class = outcomes.ship
WHERE bore = 16
```

# 50

Найдите сражения, в которых участвовали корабли класса Kongo из таблицы Ships.

```sql
SELECT battle
FROM ships
JOIN outcomes ON ship = name
WHERE class = 'kongo'
UNION
SELECT battle
FROM ships
JOIN outcomes ON ship = class
WHERE class = 'kongo'
```






