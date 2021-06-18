# lab

app.rb

```ruby
require_relative 'config/environment' 
require_relative 'models/piglatinizer.rb'#required the file my class is in
require 'pry'

class App < Sinatra::Base #I just relized that this grabs the data from '/'
  get '/' do #This where the file goes
    erb :user_input #and this is the file 
  end

  post '/piglatinize'do #once the '/' get is submited it post to
      #'/piglatinize'
  @translate = PigLatinizer.new.piglatinize(params[:user_phrase])
   erb :results #this is the name of the file we want to post to												
  end
end
#(params[:user_phrase]) is the argument, and will act like what ever argurment was anticipated.
#PigLatinizer.new.piglatinize(params[:user_phrase])
.piglatinize#is the method we want to call on our text that is ber submited to our get request
```

user_input.erb

```erb
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>title</title>
  </head>
  <body>
    <form method="POST" action="/piglatinize"> <!-- THis will post to the '/piglatinize' using the post method-->
      <h1>Pig Latinizer!</h1>
      <h3>Enter your phrase:</h3>
    <textarea name="user_phrase"></textarea>
    <input id="submit" type="submit" name="submit">
  </form>
  </body>
</html>
```

results.erb

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>

  </head>
  <body>
    <h1><%= "#{@translate}" %></h1> <!-- this is just the variable the contains what we want displayed-->

  </body>
</html>
```

piglatinizer.rb

```ruby
require 'pry'
class PigLatinizer
attr_accessor :string # this is useless

 #I decided not to intialize because PigLatinizer.new(params[:user_phrase]) is practically saying this 
    #pigLatinize.new("text from text box after submited")

def piglatinize_word(word) # or whatever word is passed in considered params[:word]
first_letter = word[0].downcase

if ["a", "e", "i", "o", "u"].include?(first_letter)
  "#{word}way"
else
consonants = []
consonants << word[0]
if   ["a", "e", "i", "o", "u"].include?(word[1]) == false
  consonants << word[1]

if   ["a", "e", "i", "o", "u"].include?(word[2]) == false
  consonants << word[2]
    end
  end
  "#{word[consonants.length..-1] + consonants.join + "ay"}"
end
end

def piglatinize(string) # PigLatinizer.new.piglatinze(params[:string_value_from_key])
  s = string.split(" ")
  b = s.map {|word| piglatinize_word(word) }
  b.join(" ")
end
# binding.pry
end

```

![](C:\Users\camer\AppData\Roaming\Typora\typora-user-images\get Digram.png)