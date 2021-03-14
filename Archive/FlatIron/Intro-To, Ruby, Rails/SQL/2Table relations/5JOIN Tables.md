## Join Tables and the "many-to-many" Relationship

A **join table** contains common fields from two or more other tables

#### Creating the Table

Since our table is creating a many-to-many relationship between cats and owners, we will call our table **cats_owners**

A **JOIN Table** for the most part is made like any other table like so:

```sqlite
--IN pets db
CREATE TABLE cats_owners (
cat_id INTEGER,
owner_id INTEGER
);
```

#### Inserting Data into the Join Table

**EX: below** uses a cats to owners relationship

First, we'll insert the Nona/Sophie relationship into our join table. Recall that Nona the cat has an `id` of `3` and Sophie the owner has an `id` of `2`.

```sqlite
INSERT INTO cats_owners (cat_id, owner_id) VALUES (3, 2);
SELECT * FROM cats_owners;
--This should return
cat_id           owner_id  
---------------  ----------
3                2  
--Now let's insert the Nona/Penny relationship into our join table:
INSERT INTO cats_owners (cat_id, owner_id) VALUES (3, 3);
cat_id           owner_id  
---------------  ----------
3                2         
3                3    
--Let's insert the appropriate row into our join table
INSERT INTO cats_owners (cat_id, owner_id) VALUES (1, 2);
cat_id           owner_id  
---------------  ----------
3                2         
3                3         
1                2    
--Let's SELECT from our join table all of the owners who are associated to cat number 3
 SELECT cats_owners.owner_id 
 FROM cats_owners 
 WHERE cat_id = 3;
owner_id       
---------------
2              
3
--Now let's SELECT all of the cats who are associated with owner number 2:
SELECT cats_owners.cat_id 
FROM cats_owners 
WHERE owner_id = 2;

cat_id         
---------------
3              
1   
```

#### Advanced Queries

```sqlite
SELECT owners.name 
FROM owners 
INNER JOIN cats_owners 
ON owners.id = cats_owners.owner_id WHERE cats_owners.cat_id = 3;
--this should retun
name           
---------------
Sophie         
Penny  
```

Let's break down the above query:

- `SELECT owners.name` - Here, we declare the column data that we want to actually have returned to us.
- `FROM owners` - Here, we specify the table whose column we are querying.
- `INNER JOIN cats_owners ON owners.id = cats_owners.owner_id` - Here, we are joining the `cats_owners` table on the `owners` table. We are telling our query to look for owners whose `id` column matches up to the `owner_id` column in the `cats_owners` table.
- `WHERE cats_owners.cat_id = 3;` - Here, we are adding an additional condition to our query. We are telling our query to look at the `cats_owners` table rows where the value of the `cat_id` column is `3`. Then, *for those rows only*, cross reference the `owner_id` column value with the `id` column in the `owners` table.

Let's take a look at a boiler-plate query that utilizes a JOIN statement to query a join table:

```sqlite
SELECT column(s)
FROM table_one
INNER JOIN table_two
ON table_one.column_name = table_two.column_name
WHERE table_two.column_name = condition;
-- EX: 
SELECT cats.name
FROM cats
INNER JOIN cats_owners
ON cats.id = cats_owners.cat_id
WHERE cats_owners.owner_id = 2;
name           
---------------
Nona           
Maru 
```

