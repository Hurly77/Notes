### INSTERING DATA 

```sqlite
INSERT INTO cats (name, age, breed) VALUES ('Maru', 3, 'Scottish Fold');
```

**Important:** Note that we *didn't specify* the "id" column name or value.

INTEGER PRIMARY KEY you don't have to specify the id column values when we insert data. Primary Key columns are auto-incrementing. As long as you have defined an id column with a data type of `INTEGER PRIMARY KEY`, a newly inserted row's id column will be automatically given the correct value.

## Selecting Data

This is where the `SELECT` statement comes in

A basic `SELECT` statement works like this:

```sqlite
SELECT [names of columns we are going to select] FROM [table we are selecting from];
```

A faster way to get data from every column in our table is to use a special selector, known commonly as the 'wildcard', `*`

```sqlite
SELECT * FROM cats;
```

You can even select more than one column name at a time. For example, try out:

```sqlite
SELECT name, age FROM cats;
```

**Top-Tip:** If you have duplicate data (for example, two cats with the same name) and you only want to select unique values, you can use the `DISTINCT` keyword. For example:

```sqlite
SELECT DISTINCT name FROM cats;
```

#### Selecting Based on Conditions: The `WHERE` Clause

```sqlite
SELECT * FROM [table name] WHERE [column name] = [some value];
ie. sqlite> SELECT * FROM cats WHERE name = "Maru";
#also you can 
SELECT * FROM cats WHERE age < 2;
```

## Updating Data

A boilerplate `UPDATE` statement looks like this:

```sql
UPDATE [table name] SET [column name] = [new value] WHERE [column name] = [value];
```

 **alter** is for altering *the actual structure of the* **table**, but **update** is for updating data in the **table**

## Deleting Data

A boilerplate `DELETE` statement looks like this:

```sqlite
DELETE FROM [table name] WHERE [column name] = [value];
```

**Top-Tip:** You can format the output of your select statements with a few helpful options:

```
.headers on       # output the name of each column
.mode column     # now we are in column mode, enabling us to run the next two .width commands
.width auto      # adjusts and normalizes column width
# or
.width NUM1, NUM2 # customize column width
```

