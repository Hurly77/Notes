#### Pre-Filling Form Values

Let's start with a vanilla form (no pre-filled values yet), using the [FormTagHelper](http://api.rubyonrails.org/classes/ActionView/Helpers/FormTagHelper.html):

```erb
<!-- app/views/people/new.html.erb //-->
 
<%= form_tag("/people") do %>
  Name: <%= text_field_tag "name" %><br>
  Email: <%= text_field_tag "email" %>
  <%= submit_tag "Create Person" %>
<% end %>
```

Here's what the HTML output will look like:

```erb
<form action="/people" accept-charset="UTF-8" method="post">
  <input name="utf8" type="hidden" value="&#x2713;" />
  <input type="hidden" name="authenticity_token" value="TKTzvQF+atT/XHG/7h48xKVdXvILdiPj83XQhn2mWBNNhvv0Oh5YfAl2LM3DlHsQjbMOFVsYEyOwj+rPaSk3Bw==" />
  Name: <input type="text" name="name" id="name" /><br />
  Email: <input type="text" name="email" id="email" />
  <input type="submit" name="commit" value="Create Person" />
</form>
```

We're working with this `Person` model:

```ruby
# app/models/person.rb
 
class Person < ActiveRecord::Base
  validates :name, format: { without: /[0-9]/, message: "does not allow numbers" }
    #validation will fail if we put numbers into the "Name" field,
  validates :email, uniqueness: true
end
```

our `create` action now looks like this:

```ruby
# app/controllers/people_controller.rb
 
  def create
    @person = Person.new(person_params)
 
    if @person.valid?
      @person.save
      redirect_to person_path(@person)
    else
      # re-render the :new template WITHOUT throwing away the invalid @person
      render :new
    end
  end
```

we can use the invalid `@person` object to **"re-fill" the usually-empty** `new` form with the user's invalid entries. 

Now, let's plug the information back into the form:

```erb
<!-- app/views/people/new.html.erb //-->
 
<%= form_tag "/people" do %>
  Name: <%= text_field_tag "name", @person.name %><br>
  Email: <%= text_field_tag "email", @person.email %>
  <%= submit_tag "Create Person" %>
<% end %>
```

 The HTML for the two field inputs used to look like this:

```html
Name: <input type="text" name="name" id="name" /><br>
Email: <input type="text" name="email" id="email" />
```

But now it will look like this:

```html
Name: <input type="text" name="name" id="name" value="Jane Developer" /><br />
Email: <input type="text" name="email" id="email" value="jane@developers.fake" />
<!--This is the same technique used to create edit/update forms.-->
```

We can also use the **same** form code for empty *and* pre-filled forms because `@person = Person.new` will create an empty model object whose attributes are all `nil`.

#### Displaying All Errors With `errors.full_messages`

The simplest way to show errors is to just spit them all out at the top of the form by iterating over `@person.errors.full_messages`. But first, we'll have to check whether there are errors to display with `@person.errors.any?`.

```erb
<% if @person.errors.any? %>
  <div id="error_explanation">
    <h2>There were some errors:</h2>
    <ul>
      <% @person.errors.full_messages.each do |message| %>
        <li><%= message %></li>
      <% end %>
    </ul>
  </div>
<% end %>
```

If the model has two errors, there will be two items in `full_messages`, which could result in the following HTML:

```html
<div id="error_explanation">
  <h2>There were some errors:</h2>
  <ul>
    <li>Name does not allow numbers</li>
    <li>Email is already taken</li>
  </ul>
</ul>
```

#### Displaying Pre-Field Errors With `errors[]`

`ActiveModel::Errors` has **field-specific** errors, If the field has errors, they will be returned in an array of strings:

```ruby
#Interacts with field like a hash
@person.errors[:name] #=> ["does not allow numbers"]
@person.errors[:email] #=> []
```

#we can conditionally "error-ify" each field in the form

```erb
<div class="field">
  <%= label_tag "name", "Name" %>
  <%= text_field_tag "name", @person.name %>
</div>
    
    <!--Rails will add a class if there are errors, but you can manually do so like this:--->

<div class="field<%= ' field_with_errors' if @person.errors[:name].any? %>">
  <%= label_tag "name", "Name" %>
  <%= text_field_tag "name", @person.name %>
</div>
```

You can override `ActionView::Base.field_error_proc` to change it to something that suits your UI. It's currently defined as this within `ActionView::Base:`.

**Note:** There is a deliberate space added in `' field_with_errors'` in the example above. If `@person.errors[:name].any?` validates to true, the goal here is to produce two class names separated by a space (`class=field field_with_errors`). Without the added space, we would get `class=fieldfield_with_errors` instead!

# The Whole Picture

By now, our full form has grown quite a bit:

```erb
<%= form_tag("/people") do %>
  <% if @person.errors.any? %>
    <div id="error_explanation">
      <h2>There were some errors:</h2>
      <ul>
        <% @person.errors.full_messages.each do |message| %>
          <li><%= message %></li>
        <% end %>
      </ul>
    </div>
  <% end %>
 
 
  <div class="field<%= ' field_with_errors' if @person.errors[:name].any? %>">
    <%= label_tag "name", "Name" %>
    <%= text_field_tag "name", @person.name %>
  </div>
 
  <div class="field<%= ' field_with_errors' if @person.errors[:email].any? %>">
    <%= label_tag "email", "Email" %>
    <%= text_field_tag "email", @person.email %>
  </div>
 
  <%= submit_tag "Create" %>
<% end %>
```

