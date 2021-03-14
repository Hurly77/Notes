1. ```sqlite
   sqlite> .tables
   cats
   ```

   **ALTER TABLE**

```sqlite
.schema
CREATE TABLE cats(
  id INTEGER PRIMARY KEY,
  name TEXT,
  age INTEGER
);
```

- Let's say we want to add a new column, `breed`, to our `cats` table.

```sqlite
ALTER TABLE cats ADD COLUMN breed TEXT;
```

**DROP TABLE**

```sqlite
DROP TABLE cats;
```

