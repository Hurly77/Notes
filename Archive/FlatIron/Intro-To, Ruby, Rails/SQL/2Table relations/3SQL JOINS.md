## What Is a JOIN?

A **SQL JOIN** clause is a way to combine rows from two or more tables, based on a common column between them.

***Relational databases*** allow us not only to store data that is interconnected, but to retrieve that data in ways that reflect that interconnectivity.

How would we craft a query that would grab us all of the cats with a particular owner, and even include information about that owner in the data returned to us by that query

```sqlite
#returns the list of cats, but now not both Tables
SELECT * FROM cats WHERE owner_id = 2;
```

## JOIN Types

| Type              | Description                                                  |
| ----------------- | ------------------------------------------------------------ |
| INNER JOIN        | Returns all rows when there is at least one match in BOTH tables |
| LEFT [OUTER] JOIN | Returns all rows from the left table, and the matched rows from the right table |
| RIGHT JOIN*       | Returns all rows from the right table, and the matched rows from the left table |
| FULL JOIN*        | Returns all rows when there is a match in ONE of the tables  |

**Note:** Unfortunately, SQLite does not support the RIGHT JOIN or the FULL OUTER JOIN clauses

The following statement **emulates** the `FULL OUTER JOIN` clause in SQLite:

```sqlite
SELECT d.type,
         d.color,
         c.type,
         c.color
FROM dogs d
LEFT JOIN cats c USING(color)
UNION ALL
SELECT d.type,
         d.color,
         c.type,
         c.color
FROM cats c
LEFT JOIN dogs d USING(color)
WHERE d.color IS NULL;
```

An **INNER JOIN** query will return *all* the rows from both tables you are querying where a certain condition is met

```sqlite
SELECT column_name(s)
FROM first_table
INNER JOIN second_table
ON first_table.column_name = second_table.column_name;

EX:
SELECT Cats.name, Cats.breed, Owners.name 
FROM Cats 
INNER JOIN Owners
ON Cats.owner_id = Owners.id;
-- leats break it down down
SELECT Cats.name, Cats.breed, Owners.name ...
--Here, we are specifying which columns from each table we want to select data from.
table_name.column_name
--Next up, we join our two tables together with our INNER JOIN keyword:
...FROM Cats INNER JOIN Owners
--Lastly, we tell our query how to connect, or join, the two tables.
...ON Cats.owner_id = Owners.id
```

1. name             breed            name
2. *---------------  ---------------  ----------*
3. Maru             Scottish Fold    mugumogu  
4. Hana             Tabby            mugumogu  
5. Nona             Tortoiseshell    Sophie  

Notice that the owner's name column is called `name` in the output above. That is because we requested the `name` column from the Owners table. 

## Use the AS Keyword to make the owner "owner_name"

### Note on INNER JOIN

When we say that an INNER JOIN returns all of the data for which a certain condition is true

JOIN condition, in this case, is the thing that our two tables are joined *on*:

```sqlite
...ON Cats.owner_id = Owners.id;
```

Any cats that have an empty `owner_id` column, will not be selected by the query.

## OUTER JOIN

How do we query information ***without*** exluding data that does not have an **owner_id** && value? **LEFT OUTER JOIN**

A **LEFT** **OUTER** **JOIN** query returns all rows from the **left**, or **first**, **table**, regardless of whether or not they may the **JOIN** condition.The query will also return the matched data from the right, or second, table.

Let's take a look at a boiler-plate LEFT OUTER JOIN:

```sqlite
SELECT column_name(s)
FROM first_table
LEFT [OUTER] JOIN second_table
ON first_table.column_name=second_table.column_name;
EX:
SELECT Cats.name, Cats.breed, Owners.name 
FROM Cats 
LEFT OUTER JOIN Owners 
ON Cats.owner_id = Owners.id;

```

## RIGHT OUTER JOIN and FULL OUTER JOIN

The RIGHT OUTER JOIN is the reverse of the LEFT OUTER JOIN

```sqlite
SELECT column_name(s)
FROM first_table
RIGHT JOIN second_table
ON first_table.column_name = second_table.column_name;
```

### FULL OUTER JOIN

FULL OUTER JOIN queries will combine the result of both a LEFT and RIGHT OUTER JOIN. 

Here's a boiler-plate example:

```sqlite
SELECT column_name(s)
FROM first_table
FULL OUTER JOIN second_table
ON first_table.column_name = second_table.column_name;
```

 In other words, it includes *all* of our data