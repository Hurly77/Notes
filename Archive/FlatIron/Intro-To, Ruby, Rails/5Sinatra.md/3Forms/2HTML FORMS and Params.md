#### Form

```html
<input type="text">
```

```html
<input type="submit">
```

#### First Form

```html
#views/food_form.erb
<form>
  <p>Your Name: <input type="text"></p>
  <p>Your Favorite Food: <input type="text"></p>
  <input type="submit">
</form>
```

#### Connecting the Form to your Sinatra App

```html
<form method="POST" action="/food">
```



- The `method` attribute tells the form what kind of request should be fired to the server when the submit button is clicked. In general, forms use POST request, because it is 'posting' data to the server.
- The `action` attribute tells the form what specific route the post request should be sent to. In this case, we're posting to a route called `/food`

Each form field `` also must define a `name` attribute. The `name` attribute of an `` defines how our application will identify each `` data.

#### Post Routes and Params

Every form needs  **how** and **where** or `POST` `action` and nedds a corresponding route in the controller file

```ruby
post '/food' do
 
end
```

**All** user **submitted data will appear** in a `params` hash accessible throughout our Sinatra controllers. The `name` attribute of an `` corresponds to a key in the `params` hash for that data.

```ruby
params = {
  :name => "Sam",
  :favorite_food => "Green Eggs and Ham"
}
```

