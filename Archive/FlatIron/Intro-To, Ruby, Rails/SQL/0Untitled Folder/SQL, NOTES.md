**literal** - is a notation for representing a fixed value in source code

**heredoc** or **here-document** - it is a section of a [source code](https://en.wikipedia.org/wiki/Source_code) file that is treated as if it were a separate file. The code the we use in SQLite3 LOOKS  LIKE THIS:

```ruby
class Klass
	def method
        sql = <<-SQL #
        SQL code
        SQL
	end
end
```