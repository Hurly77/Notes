### How Dynamic Routes Work:

Here's our array:

```ruby
all_the_medicines = [
  #<Medicine:0x007fb739b1af88 @id=1, @name="penicillin" @group="antibiotic">,
  #<Medicine:0x007fb739b1af88 @id=2, @name="advil" @group="anti-inflammatory">,
  #<Medicine:0x007fb739b1af88 @id=3, @name="benadryl" @group="anti-histamine">
]
```

The **First** thin Sinatra does is **match** the **request** to a specific **controller actoin**. It **then** **executes** the code in side the **block** like below:

```ruby
# medicines_controller.rb
get '/medicines/:id' do
  @medicine = all_the_medicines.select do |medicine|
    medicine.id == params[:id]
  end.first
  erb :'/medicines/show.html'
end
```

`GET` **matches** the `get` method in the **controller**. The `/medicines` path, **HTTP request** matches the `/medicines` path in our method. Finally, `1`, or `id` parameter **matches** the **controller's exectation for** **id prameter in place of** `:id`

#### URL Params

A **URL parameter** is a **variable** whose **values** are set **dynamically** in a pabge's **URL**, and can be **accessed** by its **corresponding controller action.**

The next thing that happens is that the `all_the_medicines` array is queried for a medicine object that has the `id` of 1. It seems to match this entry: `<Medicine:0x007fb739b1af88 @id=1, @name="penicillin" @group="antibiotic">,` The attributes from this object are assigned to the variable `@medicine`. Finally, the `@medicine` object is rendered via the `show.html.erb` template inside of the `views/medicines` directory.

That `:name` in the route **name** is just a **symbol** that will be filled in with text later. The **data** is passed from the **URL** to the controller action through an **automatically generated hash called** `params`. It is inside **controller action**, you **automatically** have **access** to this hash **through** the **variable** `params`.

the hash looks something like this:

```ruby
params = {
  :id => "1" #This is a String and NOT an Integer
}
```

**Note:** Values in params always come in as strings, and your return value for the route should also be a string (use `.to_i` and `.to_s`).

Once **inside** the **controller action**, we can access the value from the params hash, just like we would any other hash.

```ruby
params[:id]
```

# lab

```ruby
require_relative 'config/environment'

class App < Sinatra::Base

  # This is a sample static route.
  get '/hello' do
    "Hello World!"
  end

  # This is a sample dynamic route.
  get "/hello/:name" do
    @user_name = params[:name]
    "Hello #{@user_name}!"
  end

  # Code your final two routes here:
get "/goodbye/:name" do
  @user_name = params[:name]
  "Goodbye, #{@user_name}."
end

get "/multiply/:num1/:num2" do #to be dynamic need to be key
  @multiply = params[:num1].to_i * params[:num2].to_i 
  "#{@multiply}"
end

end
```

