## Overview

A complex join in SQL is also referred to as an outer join.t is referred to as "complex" simply because SQL is conducting an inner join in addition to gathering a little more information

## Difference between Inner Join and Outer Join

### Inner Join

As you may recall, an inner join is going to return only the rows from the database that match the query.

first, lets look and an inner join

```sqlite
SELECT *
FROM Teachers
INNER JOIN Students
ON Teachers.id = Students.teacher_id;
```

This query returns only the teacher with the `id = 1` because student 2 is in the first teacher's class.

**Note**: Since we're *joining* tables, running this example SQL command will return a result with both an *id* and a *teacher_id*, even though they are the same.

### Outer Join

**Outer Joins,** on the other hand, will return all of the matching rows AND all of the additional rows from the specified table.

 There are three types of outer joins: **Left Outer Join**, **Right Outer Join**, and **Full Outer Join.**

**NOTE** SQL, does not implement the full SQL standard. Specifically, SQLite does not implement RIGHT OUTER JOIN or FULL OUTER JOIN.

#### Left Outer Join

This returns the normal inner join result and also ***returns all of the rows from the left-most (i.e. first mentioned) table\***.

EX:

```sqlite
SELECT
  Teachers.id as teacher_id,
  Students.student_id
FROM Teachers
LEFT OUTER JOIN Students
ON Teachers.id = Students.teacher_id;
```

`as` name helps make the output easier to read.

#### Right Outer Join

This ***returns all of the rows from the right-most (i.e. last-mentioned) table\***

```sqlite
SELECT
  Teachers.id as teacher_id,
  Students.id as student_id
FROM Teachers
RIGHT OUTER JOIN Students
ON Teachers.id = Students.teacher_id;
```

### Full Outer Join

The full ***returns all of the rows from all the tables\***.

```sqlite
SELECT
  Teachers.id as teacher_id,
  Students.id as student_id
FROM Teachers
FULL OUTER JOIN Students
ON Teachers.id = Students.teacher_id;
```

 **returns the overlapping areas** 

![left outer join diagram](http://readme-pics.s3.amazonaws.com/Left%20Outer%20Join%20Venn%20Diagram.png)![full outer join diagram](http://readme-pics.s3.amazonaws.com/Full%20Outer%20Join%20Venn%20Diagram.png)![inner join diagram](http://readme-pics.s3.amazonaws.com/Inner%20Join%20Venn%20Diagram.png)![right outer join diagram](http://readme-pics.s3.amazonaws.com/Right%20Outer%20Join%20Venn%20Diagram.png)