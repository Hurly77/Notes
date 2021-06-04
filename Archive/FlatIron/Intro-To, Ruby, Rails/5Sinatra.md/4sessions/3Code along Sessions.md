## Sessions and Data Persistance

The **Hyper-Text Transfer Protocol (HTTP)** is, by definition, a stateless protocol. 

**HTTP**  is called a "**stateless protocol**" because a browser does not attach special meaning to a request, and consequently **does not** require the server to retain any **information** **about** a **user** or **entity** for the duration of a request.

**A session is** a hash that lives in your application in the server. 

the act of "**logging** in", *is simply the the act of having your user id stored inside the session hash*. The act of "**logging out**" *is simply the act of your user id being removed from the session hash*.

#### Using Sessions in Sinatra

`app.rb`

```ruby
configure do
    enable :sessions unless test? #enabling our application to use the SESSIONS keyword to access the session hash.
    set :session_secret, "secret" #Don't worry too much about the session secret for now
  end
```

```ruby
class App < Sinatra::Base
  configure do
    enable :sessions unless test?
    set :session_secret, "secret"
  end

  before do
    content_type :txt
  end

  get '/' do
    "Welcome to Sinatra Sessions! In this lab, we will be learning about the general principles behind a sessions cookie. In order to proceed, let's go to the '/first_exercise' path."
  end

  get '/first_exercise' do
    "Your first exercise will be to set your session key-value pair.\nIn the route: get '/set', write a line of code that sets the :foo key of the session hash equal to 'hello'.\nThen, navigate to the '/set' path."
  end

  get '/set' do
    session[:foo] = 'hello'# set the :foo key of the session hash equal to 'hello' here!
    if session[:foo] == 'hello'
      redirect '/fetch'
    else
      "Session value has not been set!"
    end
  end

  get '/fetch' do
    "You did it! session[:foo] value: #{session[:foo]}.\nMove on to Part II of this lab at '/second_exercise' "
  end

  get '/second_exercise' do
    "Your second lesson will be to set your session :id key to a value.\nIn the route: /set_session, write a line of code that sets the session[:id] equal to 1.\nThen, navigate to the '/set_session' path"
  end

  get '/set_session' do
    session[:id] = 1

    if session[:id] == 1
      # "Session ID set. It's currently set to #{session[:id]}."
      redirect '/fetch_session_id'
    else
      "Session ID has not been set!"
    end
  end

  get '/fetch_session_id' do
    "You did it! session[:id] value: #{session[:id]}.\nNow, clear the session in the '/logout' route.\nSee the readme for further instructions.\nThen, navigate to the '/logout' path."
  end

  get '/logout' do
    session.clear
    "Session has now been cleared. session content: #{session.inspect}. Continue on to the '/finish' line!"
  end

  get '/finish' do
    "Hopefully that explains a little more about the concept of sessions.\nThe session is simply a way to store user data on a temporary basis.\nIn any web application, a user ID is typically used as a session ID.\nThis is because an ID attribute of a user is a unique identifier\nthat will always be distinguishable from other user ID attributes."
  end
end
```

