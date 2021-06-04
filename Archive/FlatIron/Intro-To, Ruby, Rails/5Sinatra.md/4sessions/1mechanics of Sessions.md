### Setting Up A Session

 set up the session...

In Sinatra, we enable sessions within the controller (e.g., `app.rb`) by adding two lines in the `configure` block:

```ruby
configure do
  enable :sessions
  set :session_secret, "secret"
end
#Don't worry too much about understanding how the session_id works
```

> **IMPORTANT**: You should **never** set your `session_secret` by hand, and especially not to something so trivially simple as `"secret"`! We are doing this for the sake of demonstration this *one* time. You are advised to learn more about how to secure your sessions by following the [Using Sessions][secsin] documentation at the Sinatra home.

The first line of the configure block, `enable :sessions`, turns sessions on. The next line, `set :session_secret, "secret"`, is an encryption key that will be used to create a `session_id`. A `session_id` is a string of letters and numbers that is unique to a given user's session and is stored in the browser cookie. You can actually set your `session_secret` to anything that you want.

`session_secret` is a pretty minimal **security** feature, but the basic idea is that setting your `:session_secret` to a word that other people don't know **makes** it **harder** for **someone to create a fake session_id and hack into your site** without signing up or signing in.

### Using Sessions

 we need to set up the `session` hash to store the `user_id` in the hash during a controller action. we can simply use the `session` hash to store the user's name:

```ruby
#we haven't covered how to set up logins and logouts
get '/hey' do 
  @session = session
end
```

n the example above, we have a route, `/hey`, that gets processed by a `GET` request. Because we enabled sessions in our app, every controller action has access to the `session` hash.

We stored the `session` hash in the instance variable `@session` so that our views will have access to the session data. In this case, `@session` now looks like this:

```ruby
@session = {
  "session_id"=>  
    "dd32f512ee239ad74aa6f10c8cad37ce28d6c6922eff252ed641b1017130fe22", 
  "csrf"=> "040e9777d4dfae03bb1e6498f2a75482", 
  "tracking"=>{ 
    "HTTP_USER_AGENT"=> "e193e9e937caa9a19ca483f046281aae77d2216b", 
    "HTTP_ACCEPT_LANGUAGE"=> "66eae971492938c2dcc2fb1ddc8d7ec3196037da"
  }
}
```

You can access information from the hash just like you would any hash. `@session["session_id"]` will return `"dd32f512ee239ad74aa6f10c8cad37ce28d6c6922eff252ed641b1017130fe22"`.

You can also modify and add data to the `session` hash by adding a key-value pair:

```ruby
get '/hey' do 
  session["name"] = "Victoria"
  @session = session
end
```

### Viewing Sessions

##### Viewing in Developer Tools

Even though sessions are created on the server, you can view the contents of the `session` hash as a cookie in your browser using Developer Tools.

click on the `Application` tab. On the left side of the tools, open up the `Cookies` dropdown, click the `learn.co` cookie, and it will bring up all of the cookies loaded on the site. You can see that several of them say `Session` in the `Expires / Max-Age` column.

### Securing Your Session

As we mentioned at the beginning, you should not define you `session_secret` as we do in this lab. The most secure method is to use a secure number generator to generate a secret and to share that secret, via environment variables in the shell, to Sinatra. This is covered in [their documentation][secsin].

[secsin]: 