## Grouping and Sorting Data

## Setting up the Database

WE WILL LIKE NORMAL USE THE **pets_database.db**

cats, owners, cats_owners  with **TABLES**

### ORDER BY( )

#### Syntax

```sqlite
--ORDER BY() will automatically sort the returned values in ascending order
SELECT column_name, column_name
FROM table_name
ORDER BY column_name DESC, column_name ASC|DESC;
```

list of famous and wealthy cats

 We can do that with a basic `SELECT` statement:

```sqlite
SELECT * FROM cats WHERE net_worth > 0;
SELECT * FROM cats ORDER BY(net_worth) DESC;
--This will return:
name             age         breed          net_worth
---------------  ----------  -------------  ----------
Maru             3           Scottish Fold  1000000
Hana             1           Tabby          21800
Grumpy Cat       4           Persian        181600
Lil Bub         2           Tortoiseshell  2000000

--return the list to them with the cats sorted by net worth
SELECT * FROM cats ORDER BY(net_worth) DESC;

--This will return:
name             age         breed          net_worth
---------------  ----------  -------------  ----------
LilBub        2           Tortoiseshell  2000000
Maru             3           Scottish Fold  1000000
Grumpy Cat       4           Persian        181600
Hana             1           Tabby          21800
```

### The  **LIMIT**  Keyword

```sqlite
--return the wealthiest cat
SELECT * FROM cats ORDER BY(net_worth) DESC LIMIT 1;

--Which will return:
name             age         breed          net_worth
---------------  ----------  -------------  ----------
Lil\' Bub        2           Tortoiseshell  2000000

```

### GROUP BY( )

#### Syntax

```sqlite
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name;

--EX:
--Let's calculate the sum of the net worth of all of the cats, grouped by owner name:
SELECT owners.name, SUM(cats.net_worth)
FROM owners
INNER JOIN cats_owners
ON owners.id = cats_owners.owner_id
JOIN cats ON cats_owners.cat_id = cats.id
GROUP BY owners.name;

--This should return:
owners.name      SUM(cats.networth)
---------------  ----------
Penny            181600
Sophie           1021800
--In the above query, we've implemented two joins. First, we're joining owners and cat_owners on owners.id = cats_owners.owner_id.
owners.id  owners.name      cat_owners.cat_id  cat_owners.owner_id
---------  -----------      -----------------  -----------------
2          Sophie           2                  2
3          Penny            3                  3
2          Sophie           1                  2
--With this table, we then implement a second join with cats on cats_owners.cat_id = cats.id
--when we use the SUM(cats.net_worth) aggregator in conjunction with GROUP BY, the combination changes the way that our query behaves. Without GROUP BY, we would get a sum of the net worth of all the cats:
owners.name      SUM(cats.networth)
---------------  ----------
Sophie           1203400
--By adding GROUP BY, we now get the net_worth of all cats by owner
```

### HAVING vs WHERE  clause

Suppose we have a table called employee_bonus

To calculate the total bonus that each employee received

```sqlite
SELECT employee, SUM(bonus) from employee_bonus group by employee;
-- This would ad all the bonus by that belong to an employee

```

To find the employees who received more than $1,000 in bonuses for the year of 2007

**BAD SQL**

```sqlite
BAD SQL:
SELECT employee, SUM(bonus) FROM employee_bonus
GROUP BY employee WHERE SUM(bonus) > 1000;
```

**WHERE** clause doesn’t work with aggregates

GOOD **SQL**

```sqlite
SELECT employee, SUM(bonus) FROM employee_bonus
GROUP BY employee HAVING SUM(bonus) > 1000;
```

#### Difference between **HAVING** and **WHERE** clause

The difference between the `HAVING` and `WHERE` clause in SQL is that the `WHERE` clause can not be used with aggregates but the `HAVING` clause can. HAVING filters out groups of rows, created by 'GROUP BY' and WHERE filters out rows

- **HAVING** supports aggregate functions as it has to work with groups of rows. So for example, if there are multiple integers in a group it can filter out the groups with a low average, a high total (sum) or count how many rows are in the group.
- **WHERE** on the other hand deals with each row individually, so aggregate functions wouldn't work for what would you be aggregating.

**HAVING** is **after** GROUP BY and **WHERE** is **before** GROUP BY 

```sqlite
SELECT
FROM
JOIN
  ON
WHERE
GROUP BY
HAVING
ORDER BY
LIMIT
```

ORDER BY VS. GROUP BY

Group** by and **Order** by in SQL: **ORDER** BY alters the **order** in which items are returned. an **order** by sorts. ... **GROUP** BY will aggregate records by the specified columns which allows you to perform aggregation functions on non-grouped columns (such as SUM, COUNT, AVG, etc).

**GROUP BY** : is used to group certain records of result set based on the specified column. Suppose, if the specified column has duplicate values , then those values are combined or grouped and according to this grouping other column values are also grouped.

GROUP BY is used in conjunction with aggregate functions like SUM, MAX, MIN, AVG, COUNT etc

GROUP BY follows **WHERE** clause in **SELECT** statement and it is used before **ORDER BY clause.**

GROUP BY also sorts in ascending order by default.

**ORDER BY**: **alters** the **order** in which items are returned. an order by **sorts.**

ORDER BY allows you to sort the result set according to different criteria, such as first sort by name from a-z, then sort by the price highest to lowest.

(ORDER BY name, price DESC)

**GROUP BY** will aggregate records by the specified columns which allows you to perform **aggregation functions** on non-grouped columns (such as SUM, COUNT, AVG, etc).

a group by performs a grouping operation