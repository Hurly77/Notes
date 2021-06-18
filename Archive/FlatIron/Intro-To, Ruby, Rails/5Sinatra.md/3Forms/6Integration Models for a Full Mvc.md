#### web application that analyzes a block of text from the user

- `get '/' do`, which renders the `index.erb` page.
- `post '/' do`, which receives the form data through params and renders the results page.

## Creating a model

**create** a new **class** inside of our `models` directory that will take care of the analysis of the text.

```ruby
class TextAnalyzer
  attr_reader :text
 
  def initialize(text)
    @text = text.downcase
  end
 
  def count_of_words
    words = text.split(" ")
    words.count
  end
 
  def count_of_vowels
    text.scan(/[aeoui]/).count
  end
 
  def count_of_consonants
    text.scan(/[bcdfghjklmnpqrstvwxyz]/).count
  end
 
  def most_used_letter
    s1 = text.gsub(/[^a-z]/, '') # gets rid of spaces
    arr = s1.split('')
    arr1 = arr.uniq
    arr2 = {}
 
    arr1.map do |c|
      arr2[c] =  arr.count(c)
    end
 
    biggest = { arr2.keys.first => arr2.values.first }
 
    arr2.each do |key, value|
      if value > biggest.values.first
        biggest = {}
        biggest[key] = value
      end
    end
 
    biggest
  end
end
```

The model above has an **initializer which takes in a string** `text`, **converts it to lowercase, and saves it** **to an instance variable** `@text`. This instance variable is then **used in the four instance methods**, which provide **information** on the **block** of text in question. If we wanted to **use** this **class** on its **own**, we could do the following:

```ruby
my_text = TextAnalyzer.new("The rain in Spain stays mainly on the plain.")
my_text.count_of_words #=> 9
my_text.count_of_vowels #=> 13
my_text.count_of_consonants #=> 22
my_text.most_used_letter #=> {"n" => 6}
# we could drop this class into a Command Line or Ruby on Rails app and it would function in the exact same way.
```

## Using a model in the controller

**add** a `require_relative` statement **to** the **controller** so that this file **can** **use** this **new** **class**.At the top of `app.rb`, add `require_relative "models/text_analyzer.rb"`. We can now use`TextAnalyzer` class and invoke its `new` method

```ruby
post '/' do
  text_from_user = params[:user_text]
 
  @analyzed_text = TextAnalyzer.new(text_from_user)
 
  erb :results
end
#We can shorten this to:
post '/' do
  @analyzed_text = TextAnalyzer.new(params[:user_text])
 
  erb :results
end
```

#### Using instance variables in ERB

In our `results.erb` file, use erb tags to display the data stored in the `@analyzed_text` variable. 

# lab

### app.rb

```ruby
  post '/' do
    @analyzed_text = TextAnalyzer.new(params[:user_text])

    erb :results
  end
```

### results.erb

```erb
<h1> Your Text Analysis </h1>

<h2>Number of Words: <%= "#{@analyzed_text.count_of_words}" %></h2>

<h2>Vowels: <%= "#{@analyzed_text.count_of_vowels}" %></h2>

<h2>Consonants: <%= "#{@analyzed_text.count_of_consonants}" %></h2>

<h2>Most Common Letter: <%= "#{@analyzed_text.most_used_letter.keys[0]}" %>, used <%= "#{@analyzed_text.most_used_letter.values[0]}" %> times</h2>
```

### class method

```ruby
class TextAnalyzer
  attr_reader :text

  def initialize(text)
    @text = text.downcase
  end

  def count_of_words
    words = text.split(" ")
    words.count
  end

  def count_of_vowels
    text.scan(/[aeoui]/).count
  end

  def count_of_consonants
    text.scan(/[bcdfghjklmnpqrstvwxyz]/).count
  end

  def most_used_letter
    s1 = text.gsub(/[^a-z]/, '') # gets rid of spaces
    arr = s1.split('')
    arr1 = arr.uniq
    arr2 = {}

    arr1.map do |c|
      arr2[c] =  arr.count(c)
    end

    biggest = { arr2.keys.first => arr2.values.first }

    arr2.each do |key, value|
      if value > biggest.values.first
        biggest = {}
        biggest[key] = value
      end
    end

    biggest
  end
end
```

