## Relating Tables with Foreign Keys

This is great in Ruby, but there is no data type for arrays in SQL. You can only have `INTEGER`, `FLOAT`, and `TEXT`. 

The `id` column or  **PRIMARY KEY** for each row is a unique `INTEGER` identifier for that row.

et's say the Post "10 Ways to Pet Your Cat" is written by "Joe Burgess", and Joe's **id** is 5. We just need to add a new column to our Posts table with the **id** of the Author that it's related to. Let's call this column **author_id.**

This **author_id** column is called a "**foreign key**".

Why didn't we do the reverse? there isn't any way to store multiple items in a single column

#### Add Foreign Key 

```sqlite
ALTER TABLE cats ADD COLUMN owner_id INTEGER;
```

#### Associating

```sqlite
#creates owner
INSERT INTO owners (name) VALUES ("mugumogu");
#create association to cats 
UPDATE cats SET owner_id = 1 WHERE name = "Maru";
UPDATE cats SET owner_id = 1 WHERE name = "Hana";
#now take a look
SELECT * FROM cats WHERE owner_id = 1;
#this should return
1 | Maru | 3 | Scottish Fold | 1
2 | Hana | 1 | Tabby         | 1
```

### Establishing Foreign Key: Determining Which Table Gets a "foreign key" Column

Let's look at what would happen if we tried to add cats directly to the owners table.

Adding the first cat, "Maru", to the owner "mugumogu" would look something like this:

| id   | name     | cat_id |
| ---- | -------- | ------ |
| 1    | mugumogu | 1      |

So far so good. But what happens when we need to add a second cat, "Hana", to the same owner?

| id   | name     | cat_id1 | cat_id2 |
| ---- | -------- | ------- | ------- |
| 1    | mugumogu | 1       | 2       |

What if this owner gets *yet another cat?* We'd have to keep growing our table.

We can think about the *relationship*  in the context of a "**has many**" and "**belongs** to" *relationship.*

The thing that "**has many**" is considered to be the **parent**.The thing that "**belongs to**" we'll call the **child**

The **child** table gets the **foreign key** column, the value of which is the **primary key** of that data's/row's **parent.**

IN SHORT

CHILD = FOREIGN KEY

PARENT = PRIMARY KEY