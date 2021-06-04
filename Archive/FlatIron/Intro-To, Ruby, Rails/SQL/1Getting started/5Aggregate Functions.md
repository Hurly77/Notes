## Aggregate Functions

**agregate**- a whole formed by combining several (typically disparate) elements.

```
average, sum, count, minimum, maximum
```

- `AVG`
- `SUM`
- `COUNT`
- `MIN`
- `MAX`

### **AVG**

The average, `AVG()`, function returns the average value of a column. Here's how it works:

```sqlite
SELECT AVG(column_name) FROM table_name;
EX: SELECT AVG(net_worth) FROM cats;
```

[^note to self]: AS

```sqlite
EX:**SELECT** **AVG**(net_worth) **AS** average_net_worth **FROM** cats;
```

### **SUM**

**SUM()**, function returns the sum of all of the values in a particular column.

```sqlite
SELECT SUM(column_name) FROM table_name;
EX: SELECT SUM(column_name) FROM table_name;
```

### MIN and  MAX

The **minimum** and **maximum** aggregator functions return the minimum and maximum values 

```sqlite
SELECT MIN(column_name) FROM table_name;
SELECT MAX(column_name) FROM table_name;
EX: SELECT MIN(net_worth) FROM cats;

```

## count

The **count** function returns the number of rows that meet a certain condition.

```sqlite
SELECT COUNT(column_name) FROM table_name;
EX: SELECT COUNT(name) FROM cats;
```

COUNT(*)  will count the rows where at least one column has data in it.

```sqlite
SELECT COUNT(*) FROM cats WHERE net_worth > 1000000;
```

