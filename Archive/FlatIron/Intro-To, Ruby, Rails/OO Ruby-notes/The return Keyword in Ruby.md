- **explicit** return

- **implicit** return

- **unexpected** return s

- return in **procs** and **lambdas**

  **explicitly**  -in a clear and detailed manner, leaving no room for confusion or doubt.

  **Implicit** -implied though not plainly expressed.

  **omitted** -leave out or exclude (someone or something), either intentionally or forgetfully.

- Every block in ruby will return the value of the last line automatically
- so it's common to not use the `return` keyword

## Explicit return

Ruby provides a keyword that allows the developer to ***explicitly* stop** the execution flow of a method and return a specific value.



This keyword is named as **return**

```ruby
def explicit_return_call
  puts 'before return call'
  
  return 'return call'
  
  puts 'after return call'
end

puts explicit_return_call
```

produces

```
before return call
return call
```

Here, we can see that a call to `explicit_return_call` executes all the instructions until the call to `return 'return call'`.

When this instruction is executed the execution flow is suddenly stopped and the `'return call'` string is returned.

So, the `puts 'after return call'` is never executed.

Let’s see what happens if we call `return` without value

```ruby
def return_without_value
  return
end

return_without_value # => nil
```

The `return` keyword returns `nil` if no value is passed as argument.

## Implicit return

```ruby
def implicit_return
  if true
    42
  else
    0
  end
end

implicit_return # => 42

def rom_ebook
  'Ruby Object Model - eBook'
end

rom_ebook # => "Ruby Object Model - eBook"
```

In the `implicit_return` method, as `if true` is always evaluated as `true` (mister obvious) then the last executed instruction is `42`. So the method logically returns `42`.

As the `rom_ebook` method contains only one instruction the `'Ruby Object Model — eBook'` string is returned.

### `return` **and assignment methods**

The `return` keyword behaves differently when it has to deal with assignment methods

```ruby
class MyClass
  def x=(y)
    return 42
  end
end

p MyClass.new.x = 21 # => 21
```

Here we can see that our `return` statement is completely omitted by our method.

Instead, the `MyClass#x=` method returns the value passed as argument — in our case `21`.