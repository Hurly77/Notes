## Request Flow

![MVC Request Flow](https://s3.amazonaws.com/flatiron-bucket/readme-lessons/mvc_flow_updated.png)

## Roles and Responsibilities

### Models

### Controllers

### Views -

if you wanted to create a `div` for a set of blog posts that you want to iterate over, you can implement the following code:

```erb
<%= content_tag(:div, @post, class: "post-index-page") do %>
  <%= content_tag(:p, "#{@post.title}: #{@post.summary}") %>
<% end %>
```

Which is translated to the following HTML markup:

```html
<div id="post_42" class="post post-index-page">
  <p>My Amazing Blog Post: With an incredible summary</p>
</div>
```