  POST /team
    does not return Sinatra error page (FAILED - 2)
    displays the basketball team name in the browser (FAILED - 3)
    displays the coach's name in the browser (FAILED - 4)
    displays the point guard's name in the browser (FAILED - 5)
    displays the shooting guard's name in the browser (FAILED - 6)
    displays the power forward's name in the browser (FAILED - 7)
    displays the center's name in the browser (FAILED - 8)

## Instructions

1. Run `bundle install`
2. Run `shotgun`
3. Make a form

Create a route that responds to a GET request at `/newteam`. Add a form to the `newteam.erb` template and render it in the GET `/newteam` route.

The form should have fields for: Team name ('name') Coach ('coach') Point Guard ('pg') Shooting Guard ('sg') Power Forward ('pf') Small Forward ('sf') Center ('c')

It should look something like this:

![form for basketball team](https://curriculum-content.s3.amazonaws.com/web-development/Sinatra/basketball-form.png)

When creating your form, your "Submit" button will need to be identified by an `id` attribute with value of "Submit". We're telling this to you now because our test framework, Capybara, requires buttons to be [findable by an `id`, `title`, or `value` attribute](http://www.rubydoc.info/gems/capybara/Capybara%2FNode%2FActions%3Aclick_button).

1. Handle form submission

Create a route that responds to a POST request at `/team` Have the form send a POST request to this route. Upon submission, pass the submitted data to the `team.erb` template.

1. Final Output

Update the `team.erb` template so when you post to this form, it displays the name of the team and each member of the team.

chYour view should display something like this:

![completed form](https://curriculum-content.s3.amazonaws.com/web-development/Sinatra/basketball-results.png)

```ruby
class App < Sinatra::Base

  get '/newteam' do
    erb :newteam
  end

  post '/team' do
      
    @name = params["name"]
    @coach = params["coach"]
    @pg = params["pg"]
    @sg = params["sg"]
    @sf = params["sf"]
    @pf = params["pf"]
    @center = params["c"]

    erb :team
  end

end


```

```html
<!DOCTYPE html>
<html>
 <head>
  <meta charset="UTF-8">
  <title>Basketball Team Signup</title>
 </head>
 <body>
   <form method="POST" action="/team"> 
     <h1>Create a Basketball Team!</h1>
     <p>Team Name: <input type="text" name="name"></p> #name attribute equals the key
     <p>Coach: <input type="text" name="coach"></p>
     <p>Point Guard: <input type="text" name="pg"></p>
     <p>Shooting Guard: <input type="text" name="sg"></p>
     <p>Small Forward: <input type="text" name="sf"></p>
     <p>Power Foward: <input type="text" name="pf"></p>
     <p>Center: <input type="text" name="c"></p>
     <input type="submit" name="submit" value="Submit" id="submit" /> #submit value has to be capitilized
   </form>
 </body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Basketball Team</title>
</head>
<body>
 <h1>Team Name: <%= "#{@name}" %></h1> 
 <h2>Coach: <%= "#{@coach}" %></h2>
 <h2>Point Guard: <%= "#{@pg}" %></h2>
 <h2>Shooting Guard: <%= "#{@sg}" %></h2>
 <h2>Small Forward: <%= "#{@sf}" %></h2>
 <h2>Power Forward: <%= "#{@pf}" %></h2>
 <h2>Center: <%= "#{@center}" %></h2>
</body>
</html>
```

