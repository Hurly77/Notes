### Note on Testing

```ruby
before do
  @db = SQLite3::Database.new(':memory:')
  @sql_runner = SQLRunner.new(@db)
  @sql_runner.execute_create_file
end
```

Before each test two important things happen.

First, a new in-memory database is created. Why do we do this? Let's say we run our tests and they add ten items to our database. If we did not use an in-memory store, those would be in there forever. This way our database gets thrown out after every running of the tests.

Next, a new `SqlRunner` class is created. The `SqlRunner` class lives in your `bin` directory and was created to help connect to the database.

- Mr. Chocolate
- Rowdy
- Tabitha
- Sergeant Brown
- Melissa
- Grinch
- Wendy