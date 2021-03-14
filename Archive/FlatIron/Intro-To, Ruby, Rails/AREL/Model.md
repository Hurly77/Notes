```ruby
class Boat < ActiveRecord::Base
  belongs_to  :captain
  has_many    :boat_classifications
  has_many    :classifications, through: :boat_classifications

  def self.first_five
    limit(5).to_a
  end

  def self.dinghy
    # where("length < 20")
   where("length < 20")
  end

  def self.ship
    # where("length >= 20")
    where("length >= 20")
  end

  def self.last_three_alphabetically
    # all.order(name: :desc).limit(3)
    all.order(name: :desc).limit(3)
      #calls the array by name and in desc, and only calls the first three
  end

  def self.without_a_captain
    # where(captain_id: nil)
    where(captain_id: nil)
  end

  def self.sailboats
    includes(:classifications).where(classifications: { name: 'Sailboat' })
    
  end

  def self.with_three_classifications
    # This is really complex! It's not common to write code like this
    # regularly. Just know that we can get this out of the database in
    # milliseconds whereas it would take whole seconds for Ruby to do the same.
    #
    joins(:classifications).group("boats.id").having("COUNT(*) = 3").select("boats.*")
  end

  def self.non_sailboats
    # where("id NOT IN (?)", self.sailboats.pluck(:id))
    where("id NOT IN (?)", self.sailboats.pluck(:id)
        #pluck can be used to query single or multiple columns from the underlying table of a model. It 		#accepts a list of column names as argument and returns an array of values of the specified 			#columns with the corresponding data type.
   end

  def self.longest
    # order('length DESC').first
    order('length DESC').first
  end
end
```

```ruby
class Captain < ActiveRecord::Base
  has_many :boats

  def self.catamaran_operators
     includes(boats: :classifications).where(classifications: {name: "Catamaran"})
  end

  def self.sailors
     includes(boats: :classifications).where(classifications: {name: "Sailboat"}).distinct
  end

  def self.motorboat_operators
     includes(boats: :classifications).where(classifications: {name: "Motorboat"})
  end

  def self.talented_seafarers
     where("id IN (?)", self.sailors.pluck(:id) & self.motorboat_operators.pluck(:id))
  end

  def self.non_sailors
     where.not("id IN (?)", self.sailors.pluck(:id))
  end

end
```

```ruby
class Classification < ActiveRecord::Base
  has_many :boat_classifications
  has_many :boats, through: :boat_classifications

  def self.my_all
     all
  end

  def self.longest
     Boat.longest.classifications
      #this just method chains with a method we wrote in Boat model
  end

end
```

#### [2.2 Array Conditions](https://guides.rubyonrails.org/active_record_querying.html#array-conditions)

Now what if that number could vary, say as an argument from somewhere? The find would then take the form:

```ruby
Client.where("orders_count = ?", params[:orders])
```

Active Record will take the first argument as the conditions string and any additional arguments will replace the question marks `(?)` in it.

If you want to specify multiple conditions:

```ruby
Client.where("orders_count = ? AND locked = ?", params[:orders], false)
```

In this example, the first question mark will be replaced with the value in `params[:orders]` and the second will be replaced with the SQL representation of `false`, which depends on the adapter.

This code is highly preferable:

```ruby
Client.where("orders_count = ?", params[:orders])
```

to this code:

```ruby
Client.where("orders_count = #{params[:orders]}")
```

because of argument safety. Putting the variable directly into the conditions string will pass the variable to the database **as-is**. This means that it will be an unshaped variable directly from a user who may have malicious intent. If you do this, you put your entire database at risk because once a user finds out they can exploit your database they can do just about anything to it. Never ever put your arguments directly inside the conditions string.

### [13 Eager Loading Associations](https://guides.rubyonrails.org/active_record_querying.html#eager-loading-associations)

Eager loading is the mechanism for loading the associated records of the objects returned by `Model.find` using as few queries as possible.

**N + 1 queries problem**

Consider the following code, which finds 10 clients and prints their postcodes:

```ruby
clients = Client.limit(10)
 
clients.each do |client|
  puts client.address.postcode
end
```

This code looks fine at the first sight. But the problem lies within the total number of queries executed. The above code executes 1 (to find 10 clients) + 10 (one per each client to load the address) = **11** queries in total.

**Solution to N + 1 queries problem**

Active Record lets you specify in advance all the associations that are going to be loaded. This is possible by specifying the `includes` method of the `Model.find` call. With `includes`, Active Record ensures that all of the specified associations are loaded using the minimum possible number of queries.

Revisiting the above case, we could rewrite `Client.limit(10)` to eager load addresses:

```ruby
clients = Client.includes(:address).limit(10)
 
clients.each do |client|
  puts client.address.postcode
end
```

