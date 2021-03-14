the `form_tag` helper method allows us to **automatically generate HTML** form code and integrate data to both **auto fill the values** as well as have the form **submit data** that the controller can use to either **create or update a record in the database.**

##### Issues with using `form_tag`

Before we get into the benefits and features of the `form_for` method, let's first discuss some of the key drawbacks to utilizing `form_tag`:

- Our form must be **manually passed to the route** where the form parameters will be submitted
- The **form has no knowledge of the form's goal**; it doesn't know if the form is meant to **create or update** a record
- You're f**orced to have duplicate code** throughout the form; it's hard to adhere to DRY principles when utilizing the `form_tag`

##### Difference between `form_for` and `form_tag`

Difference between `form_for` and `form_tag`

- the `form_for` **method accepts the instance of the model as an argument.** Using this argument, `form_for` **is able** to make a bunch of **assumptions for you**.
- `form_for` **yields an object of** class `FormBuilder`
- `form_for` **automatically knows the standard route** (it follows RESTful conventions) for the form data as opposed to having to manually declare it
- `form_for` **gives the option to dynamically change the** `submit` button **text** (this comes in very handy when you're using a form partial and the `new` and `edit` pages will share the same form, but more on that in a later lesson)

###### **NOTE**

A good rule of thumb for **when to use one approach over the other** is below:

- Use `form_for` when your form is directly connected to a model. Extending our example from the introduction, this would be our Hamster's profile edit form that connects to the profile database table. This is the most common case when `form_for` is used
- Use `form_tag` when you simply need an HTML form generated. Examples of this would be: a search form field or a contact form

##### Implementation of `form_for`

```erb
<% # app/views/posts/edit.html.erb %>
<h3>Post Form</h3>
 
<%= form_tag post_path(@post), method: "put" do %>
<!--we can remove the _path(arg) because form_for will auto set these for us-->
  <label>Post title:</label><br>
  <%= text_field_tag :title, @post.title %><br>
 
  <label>Post description</label><br>
  <%= text_area_tag :description, @post.description %><br>
 
  <%= submit_tag "Submit Post" %>
<% end %>
```

It's smart enough to **know** that because it's dealing with a pre-existing record you want to utilize `PUT` over `POST`.

```erb
<h3>Post Form</h3>
 
<%= form_for(@post) do |f| %>
<!--Allows us to dynamically assign form field elements to each of the @post's dada attributes-->
  <label>Post title:</label><br>
  <%= f.text_field :title %><br>
<!--along with auto filling the calues for eachfield, the ActionView functionality we get access to FormBuilder module in Rails-->
 
  <label>Post description</label><br>
  <%= f.text_area :description %><br>
<!--passing in the attribute as a sym auto tells the form field what model attr is to be associated with-->
  <%= f.submit %>
<!--we can now just put f.submit-->
<% end %>
```

Because `form_for` is bound directly with the `Post` model, we need to pass the model name into the Active Record `update` method in the controller. 

```ruby
def update
@post.update(title: params[:title], description: params[:description])
redirect_to post_path(@post)
end

def update
@post.update(params.require(:post).permit(:title, :description))
redirect_to post_path(@post)
end
#we need to require the post model because the old form...
{
  "title": "My Title",
  "description": "My description"
}
#was not nested like the new form is
{
  "post": {
            "title": "My Title",
            "description": "My description"
          }
}
```

**But** Rails wants us to **be conscious** of which **attributes** we allow to be **updated in our database**, so **we must** also `permit` the `title` and `description` in the nested hash

"***this will allow ActiveRecord to use mass assignment without trouble.***"*