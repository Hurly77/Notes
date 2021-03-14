***note: output files whit dir***

**ORDER BY**

This modifier allows us to order the table rows returned by a certain `SELECT` statement. 

```sqlite
SELECT column_name FROM table_name ORDER BY column_name ASC|DESC;
EX: sqlite> SELECT * FROM cats ORDER BY age
sqlite> SELECT * FROM cats ORDER BY age DESC;
```

**LIMIT**

`LIMIT`- is used to determine the number of records you want to return from a dataset. 

For example:

```sqlite
SELECT * FROM cats ORDER BY age DESC LIMIT 1;
##returns only the first cat or oldest
```

### BETWEEN

between can select in a rage such as 3-8

```sqlite
SELECT column_name(s) FROM table_name WHERE column_name BETWEEN value1 AND value2;
```

NULL

NULL - allows us to add data with missing values

```sqlite
INSERT INTO cats (name, age, breed) VALUES (NULL, NULL, "Tabby");

```

**COUNT** 

**count** - is a **SQL aggregate** function,  and will count the number of records that meet certain condition

**SQL aggregate functions** are SQL statements that retrieve minimum and maximum values from a column, get the average of a column's values, or count a number of records that meet certain conditions.

standard SQL query using `COUNT`

```sqlite
 "SELECT COUNT([column name]) FROM [table name] WHERE [column name] = [value]"
 EX:SELECT COUNT(owner_id) FROM cats WHERE owner_id = 1;
```

### GROUP BY 

GROUP BY Like its name suggests, it groups your results by a given column.

```sqlite
EX : SELECT breed, COUNT(breed) FROM cats GROUP BY breed;
```

### Note on `SELECT`

```sqlite
SELECT name FROM cats;
#and
SELECT cats.name FROM cats;
```

Both return:

```
name      ----------
Maru      
Hana      
Lil\' Bub Moe       
Patches    
```

TO SELECT MULTIPLE TABLES

```sqlite
SELECT cats.name, dogs.name FROM cats, dogs;
```

