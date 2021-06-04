The complete `params` object for creating a `Person` will look like the following. 

```ruby
{
  :person => {
    :name => "Avi",
    :addresses_attributes => {
      "0" => {
        :street_address_1 => "33 West 26th St",
        :street_address_2 => "Apt 2B",
        :city => "New York",
        :state => "NY",
        :zipcode => "10010",
        :address_type => "Work"
      },
      "1" => {
        :street_address_1 => "11 Broadway",
        :street_address_2 => "2nd Floor",
        :city => "New York",
        :state => "NY",
        :zipcode => "10004",
        :address_type => "Home"
      }
    }
  }
}
```

We're going to use `accepts_nested_attributes_for` and the `fields_for` FormHelper.

modify our `Person` model to include an `accepts_nested_attributes_for :addresses` line

```RUBY
class Person < ActiveRecord::Base
  has_many :addresses
  accepts_nested_attributes_for :addresses
 
end
```

Now open up `rails c` and run our `addresses_attributes` method that was created for us by `accepts_nested_attributes_for`.

```ruby
2.2.3 :018 > new_person = Person.new
 => #<Person id: nil, name: nil, created_at: nil, updated_at: nil>
 
2.2.3 :019 > new_person.addresses_attributes={"0"=>{"street_address_1"=>"33 West 26", "street_address_2"=>"Floor 2", "city"=>"NYC", "state"=>"NY", "zipcode"=>"10004", "address_type"=>"work1"}, "1"=>{"street_address_1"=>"11 Broadway", "street_address_2"=>"Suite 260", "city"=>"NYC", "state"=>"NY", "zipcode"=>"10004", "address_type"=>"work2"}}
 => {"0"=>{"street_address_1"=>"33 West 26", "street_address_2"=>"Floor 2", "city"=>"NYC", "state"=>"NY", "zipcode"=>"10004", "address_type"=>"work1"}, "1"=>{"street_address_1"=>"11 Broadway", "street_address_2"=>"Suite 260", "city"=>"NYC", "state"=>"NY", "zipcode"=>"10004", "address_type"=>"work2"}}

2.2.3 :020 > new_person.save
```

we have a method called `addresses_attributes=`. You didn't write that; `accepts_nested_attributes_for` wrote that. Then when we called `new_person.save` it created both the `Person` object and the two `Address` objects. Boom!

Now, we just need to get our form to create a `params` hash like that. Easy Peasy. We are going to use `fields_for` to make this happen.

```erb
# app/views/people/new.html.erb
 
<%= form_for @person do |f| %>
  <%= f.label :name %>
  <%= f.text_field :name %><br>
 
  <%= f.fields_for :addresses do |addr| %>
    <%= addr.label :street_address_1 %>
    <%= addr.text_field :street_address_1 %><br>
 
    <%= addr.label :street_address_2 %>
    <%= addr.text_field :street_address_2 %><br>
 
    <%= addr.label :city %>
    <%= addr.text_field :city %><br>
 
    <%= addr.label :state %>
    <%= addr.text_field :state %><br>
 
    <%= addr.label :zipcode %>
    <%= addr.text_field :zipcode %><br>
 
    <%= addr.label :address_type %>
    <%= addr.text_field :address_type %><br>
  <% end %>
 
 <%= f.submit %>
<!--
I'm not entirly sure but I think because in our PeopleController we have:
	"
	@person = Person.new
	@person.addresses.build(adresses: 'work')
	@person.addresses.build(adresses: 'home')
	"
I belive this is why it two forms for adresses is displayed on the screen 
wich can have two Address types home and work
-->
<% end %>
```

## Creating stubs

We're asking Rails to generate `fields_for` each of the `Person`'s addresses. However, when we first create a `Person`, they have no addresses. Just like `f.text_field :name` will have nothing in the text field if there is no name, `f.fields_for :addresses` will have no address fields if there are no addresses.

We'll take the most straightforward way out: when we create a `Person` in the `PeopleController`, we'll add two empty addresses to fill out. The final controller looks like this:

```ruby
class PeopleController < ApplicationController
  def new
    @person = Person.new
    @person.addresses.build(address_type: 'work')
      #not one hundred percent sure but I beileve the build method is to allow you to display the object with out recording it to the database. Where as new just intializes and object so you can then latter save it.
    @person.addresses.build(address_type: 'home')
  end
 
  def create
    person = Person.create(person_params)
    redirect_to people_path
  end
 
  def index
    @people = Person.all
  end
 
  private
 
  def person_params
    params.require(:person).permit(:name)
  end
end
```

 We have new `params` keys, which means we need to modify our `person_params` method to accept them. Your `person_params` method should now look like this:

```ruby
def person_params
  params.require(:person).permit(
    :name,
    addresses_attributes: [
      :street_address_1,
      :street_address_2,
      :city,
      :state,
      :zipcode,
      :address_type
    ]
  )
    #my reasoning is because you have a nested hash and permit only allow scalars which are singular data types, that you have to create an array with several attributes and if any where arrays themselves you'd have to map them to empty arrays.
end
```

## Avoiding duplicates

One situation **we can't use** `accepts_nested_attributes_for` is when we want to **avoid duplicates** of the row we're creating.

We would want `Artist` rows to be unique, so that `Artist.find_by(name: 'Tori Amos').songs` returns what we'd expect

 we'll need to use `find_or_create_by` in our `artist_attributes=` method:

```ruby
# app/models/song.rb
 
class Song < ActiveRecord::Base
  def artist_attributes=(artist)
    self.artist = Artist.find_or_create_by(name: artist[:name])
      #This looks up existing artists by name
      #If no matching artist is found, one is created
    self.artist.update(artist)
      #Then we update the artist's attributes with the ones we were given.
  end
end
#Keep in mind, too, that setter methods are useful for more than just avoiding duplicates –– that's just one domain where they come in handy.
```

## Note 

that `accepts_nested_attributes_for` and setter methods (e.g., `artist_attributes=`) **aren't** necessarily **mutually exclusive**. It's important to evaluate the needs of your specific use case and choose the approach that makes the most sense