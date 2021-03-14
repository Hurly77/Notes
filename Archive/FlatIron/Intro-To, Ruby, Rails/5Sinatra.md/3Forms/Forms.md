## aThe MVC Fram

![Web Application Stack and Tests](https://dl.dropboxusercontent.com/s/k2ypcn86btb6ajo/2015-09-29%20at%204.14%20PM.png)

**Database:** Databases persist or save data from our application.

**Models: ** **Models** *provide* an object-oriented **abstraction** or **metaphor** for the data in our application. Our **models** do the job of **interact**ing with the database for us. Our **model** can **talk** to the **database** by **asking** for certain **data** and **using** that **data** to **create a new instance of our class**. We can then interact with that data by using the methods and properties of the instance of that class.

**Controllers:**  Controllers **provide** the main **interface** and application logic. They **deal with** things like: "What **data** should I **show** a **user** in response to certain input **from** that **user**" or "What **HTML** should be **sent** to the **user** when they visit the /about page?". In large scale MVC applications, controllers are represented by classes, but really, lots of `bin` files could be considered controllers.

**Views:** Views **present** **information** to the **user**. Any code that is responsible for presenting data or **output to the user**, from methods that use a bunch of `puts`, to HTML, to ERB templates, could be considered a View. In web applications, Views are generally represented by ERB templates that generate HTML.

**User/Browser:** The top of our application pyramid is finally the user. Whether describing the people **using our application,** the interface they use such as a Command Line, Voice, or HTML, or the program they use to even access our application, such as a Browser like Chrome or Safari, or a Native app, our application is responsible for delivering the user an experience on some sort of pre-existing platform.

#### **The Three Levels of Testing**



##### Unit Tests

Unit tests test the models in our application and how they interact with our database.

##### Controller Tests

Controller tests test that the code responsible for delivering the appropriate data to a user is working properly

##### Integration Tests

Integration tests are the highest-level test, and they are closest to describing how a user will actually interact with our application.

## Integration Testing with Capybara

##### What is Capybara?

**Capybara** is a **library** of code that we can include in our application via the Capybara **gem**

##### Capybara Setup

```ruby
# Load RSpec and Capybara
require 'rspec'
require 'capybara/rspec'
require 'capybara/dsl'
 
# Configure RSpec
RSpec.configure do |config|
  # Mixin the Capybara functionality into Rspec
  config.include Capybara::DSL
  config.order = 'default'
end
 
# Define the application we're testing
def app
  # Load the application defined in config.ru
  Rack::Builder.parse_file('config.ru').first
end
 
# Configure Capybara to test against the application above
Capybara.app = app
```

#### Testing our Application



##### Capybara `visit`

The `visit` method **navigates** the test's browser to a specific **URL**. It is equivalent to a **user** typing a **URL into their browser's location bar**. The argument it accepts is a string for the URL you want to test. Since we want to test our `'/'` root URL, we say `visit '/'`, and Capybara will load that page within our test



##### Capybara `page`

The `page` method in Capybara exposes the "session" or "browser" that is conceptually (and literally) being used during the test. The `page` method gives you a `Capybara::Session` object that represents the browser page the user would actually be looking at if they navigated to `'/'` (or whichever route was last passed to `visit`).



File: `./config.ru`

```ruby
require 'sinatra'
 
require_relative './app'
 
run Application
```

In a basic application like this example, our `config.ru` `require`s the `sinatra` gem. It then `require_relative`s the required file `app.rb` that defines our main `Application` controller. Finally, we `run` our `Application`



File: `./app.rb`

```ruby
class Application < Sinatra::Base#transform the standard Ruby class into a Sinatra controller.
  get '/' do #respond to HTTP GET requests to /
    erb :index #rendering the designated ERB template or HTML.
  end
end
```

`./app.rb` is our main application file, defining the controller that will power this web application. 

Within our `Application` controller, we use the Sinatra DSL(**D**omain **S**pecific **L**anguage), to create a `GET` route at the `/` URL.



File: `views/index.erb`

```erb
<h1>Welcome!</h1>
```

The final piece of the puzzle is a `config.ru` file to put everything together and start our Sinatra application. This is the file `shotgun` or `rackup` will read to start your local application server, and it's also the file our test suite is using to define our application –– remember `Rack::Builder.parse_file('config.ru').first`?

**Note**: *If you're getting an error about conflicting gems, you may need to run `bundle exec rspec` (or `bundle exec learn`). Prepending a command with `bundle exec` forces your terminal to run the process with the version of the gems specified in the gem file rather than just using the most recent versions on your machine. If this still does not fix the error, try `bundle update`*



Edit: `spec/application_integration_spec.rb`

```ruby
require 'spec_helper'
 
describe "GET '/' - Greeting Form" do
  # Code from previous example
  it 'welcomes the user' do
    visit '/'#we tell Capybara to visit the page at '/'
    expect(page.body).to include("Welcome!")#we set some expectations against the page, HTML element that matches the form tag.
  end
 
  # New test
  it 'has a greeting form with a user_name field' do
    visit '/'
 
    expect(page).to have_selector("form") #do you have HTML that matches the following selector
    expect(page).to have_field(:user_name)#expect the page to have a form field called user_name.
  end
end
```

**Capybara** `page` objects respond to methods that relate intimately to HTML and the **DOM** (Document Object Model) that defines web applications.

You can **literally** ask the `page` object: "Hey, do you have HTML that matches the following selector?" Pretty amazing, right?



```ruby
describe "POST '/greet' - User Greeting" do
  it 'greets the user personally based on their user_name in the form' do
    visit '/' load #the form into the page object.
 
    fill_in(:user_name, :with => "Avi") Capybara #method fill_in to fill in the input field user_name with 'Avi'.
    click_button "Submit" #That HTML interaction, submitting a form, will trigger a new HTTP request in the Capybara session and page object.
 
    expect(page).to have_text("Hi Avi, nice to meet you!")#expect the page to have_text "Hi Avi, nice to meet you!"
  end
end
```

This new test is trying to **mimic** what a **user** should **see** when they visit the greeting, Because of the amazing `RSpec` DSL(**D**omain **S**pisific **L**anguage) mixed in with `Capybara`, our test is able to clearly and simply describe that behavior.

`have_text` is **another** really friendly and semantic **Capybara** **matcher** for testing **HTML** text value **explicitly**.

We need to **teach Sinatra** **application** to respond to `POST` requests to `/greet`.

Edit: `./app.rb`

```ruby
class Application < Sinatra::Base
  # Old route from previous tests
  get '/' do 
    erb :index
  end
 
  # New route to respond to the form submission
  post '/greet' do  #we create a response for requests to POST '/greet'
    erb :greet
  end
end
```

The next step is to build our view in `views/greet.erb`. The point of this view is to use the data the user submitted in the previous form within our HTML to produce a personalized greeting for the user.



File: `views/greet.erb`

```erb
<h1>Hi <%= params[:user_name] %>, nice to meet you!</h1>
```

If you are unfamiliar with the `params` object and how it relates to form and inputs, that's totally fine. The TL;DR is that all the information the user submitted in the form is available to your code within a hash named `params`. `params[:user_name]` returns the text the user typed into the form input field `user_name`. `<%= params[:user_name] %>` uses ERB's embedded ruby to dynamically insert the value of `params[:user_name]` into the HTML of the response.