### What are Routes?

**Routes** are the part of an application that connect **HTTP** requests to a specific **method** in your application code built to handle responding to such a request (that part of code is called a Controller Action).

When the user clicks on link, it directs the browser via the link's `HREF` attribute to the URL "/" must trigger the method in your application built to load and send an HTML response

#### How Do Routes Work?

In Sinatra, a route is an HTTP method paired with a URL-matching pattern in the controller would look like this:

 

```ruby
get '/medicines' do
    # some code to get all the medicines
    # some code to render the correct HTML page
 end
#OR YOU COULD WRITE IT LIKE THIS.
get('/medicines') { some code }
```

Once the request has been matched to the route in the controller called **controller action**, then it executes the code inside of the controller action's block

```ruby
# medicines_controller.rb
 
get '/medicines' do
  @medicines = Medicine.all
 
  erb :'medicines/index.html.erb'
end
```

This Line `erb :'/medicines/index.html.erb` line of code identifies a file that contains a combination of HTML and Ruby code

The `get` , or the `post`, or `delete` methods for that matter, will be invoked if Sinatra matches the HTTP method (`GET`, `POST`, etc) *and* the URL, in this case `'/medicines'`, to a route defined in the controller.