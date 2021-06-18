####   Substitution Tags

opens with `<%=` and closes with `%>`.  Inside of these tags, you can write **any** valid Ruby code that you want.

 In `index.erb`, add the following code, which should render in the browser as `I love Ruby!!`:

```erb
<%= "I love " + "Ruby!!" %>
```

```erb
<%= 1 + 1 %>
```

#### Scripting Tags

Scripting tags open with `<%` and close with `%>`. They evaluate –– but do not actually display –– Ruby code. Add the following lines of code to `index.erb`:

```erb
<% if 1 == 2 %>
  <p>1 equals 2.</p>
<% else %>
  <p>1 does not equal 2.</p>
<% end %
```

content based on whether or not a user is logged in. 

```erb
<% if logged_in? %>
  <a href="/logout">Click here to Log Out</a>
<% else %>
  <a href="/login">Click here to Log In</a>
<% end %>
```

### Iteration

We can also use iteration to manage lists of items. For instance, given an array `squares = [1,4,9,16]`:

```erb
<ul>
  <% squares = [1, 4, 9, 16] %>
  <% squares.each do |square| %>
    <li><%= square %></li>
  <% end %>
</ul>
```

Would produce:

```erb
<ul>
  <li>1</li>
  <li>4</li>
  <li>9</li>
  <li>16</li>
</ul>
```

print out all of the wall posts on a given profile page

```erb
<% wall_posts = ["First post!", "Second post!", "Hello, it's your mother. Why don't you ever call me?"] %>
```

Here, we're defining a variable called `wall_posts` and assigning its value to an array of strings. 

```erb
<ul>
  <% wall_posts.each do |post| %>
    <li><%= post %></li>
  <% end %>
</ul>
```

This should display:

```erb
<ul>
  <li>First Post!</li>
  <li>Second Post!</li>
  <li>Hello, it's your mother. Why don't you ever call me?</li>
</ul>
```

