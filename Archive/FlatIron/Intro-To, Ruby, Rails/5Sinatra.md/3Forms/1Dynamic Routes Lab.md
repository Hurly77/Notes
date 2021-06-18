| **case** | Starts a case statement definition. Takes the variable you are going to work with. |
| -------- | ------------------------------------------------------------ |
| **when** | Every condition that can be matched is one when statement.   |
| **else** | If nothing matches then do this. Optional.                   |

```ruby
require_relative 'config/environment'

class App < Sinatra::Base
  get '/reversename/:name' do
    @reversename = params[:name].reverse
    "#{@reversename}"
  end

get '/square/:number' do
@square = params[:number].to_i * params[:number].to_i
"#{@square}"
end

get '/say/:number/:phrase' do
  num = params[:number].to_i
  phrase = params[:phrase]

  @say =  phrase * num
  "#{@say%20}"
end

get '/say/:word1/:word2/:word3/:word4/:word5' do
 @word1 = params[:word1]
 @word2 = params[:word2]
 @word3 = params[:word3]
 @word4 = params[:word4]
 @word5 = params[:word5]


"#{@word1} #{@word2} #{@word3} #{@word4} #{@word5}."
end

get '/:operation/:number1/:number2' do
  @op = params[:operation]
  @num1 = params[:number1].to_i
  @num2 = params[:number2].to_i
  case @oper #the case stament alows you to work with range
    when "subtract"
      (@num1 - @num2).to_s
    when "add"
      (@num1 + @num2).to_s
    when "multiply"
      (@num1 * @num2).to_s
    when "divide"
      (@num1 / @num2).to_s
  end
end
end
```

