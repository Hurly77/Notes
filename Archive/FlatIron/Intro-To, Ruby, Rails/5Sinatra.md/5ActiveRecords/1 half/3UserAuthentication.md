## User Authorization: Using Sessions

#### Logging  In

1. User visits the login page and fills out a form with their email and password. They hit 'submit' to `POST` that data to a controller route.
2. That controller route accesses the user's email and password from the `params` hash. That info is used to find the appropriate user from the database with a line such as `User.find_by(email: params[:email], password: params[:password])`. **Then, that user's ID is stored as the value of `session[:user_id]`.**
3. As a result, we can introspect on the `session` hash in *any other controller route* and grab the current user by matching up a user ID with the value in `session[:user_id]`. That means that, for the duration of the session (i.e., the time between when someone logs in to and logs out of your app), the app will know who the current user is on every page.

##### A Note On Password Encryption

For the time being, we will simply store a user's password in the **database in its raw** form However, that is not safe! In an upcoming lesson, we'll learn about password **encryption**

##### Logging Out

The action of 'logging out' is really just the action of clearing all of the data, including the user's ID, from the `session` hash

### User Registration

A new user submits their information (for example, their name, email, and password) via a form. When that form gets submitted, a `POST` request is sent to a route defined in the controller. That route will have code that does the following:

1. gets the new user's name, email, and password from the `params` hash.
2. Uses that info to create and save a new instance of `User`. For example: `User.create(name: params[:name], email: params[:email], password: params[:password])`.
3. Signs the user in once they have completed the sign-up process. It would be annoying if you had to create a new account on a site and *then* sign in immediately afterwards. So, in the same controller route in which we create a new user, we set the `session[:user_id]` equal to the new user's ID, effectively logging them in.
4. Finally, we redirect the user somewhere else, such as their personal homepage.

## Project Structure

### Our Project

Our file structure looks like this:

```
-app
  |- controllers
      |- application_controller.rb
  |- models
      |- user.rb
  |- views
      |- home.erb
      |- registrations
          |- signup.erb
      |- sessions
          |- login.erb
      |- users
          |- home.erb
-config
-db
-spec
...
```

### The `app` Folder

The `app` folder contains the models, views and controllers that make up the core of our Sinatra application. Get used to seeing this setup. It is conventional to group these files under an `app` folder.

#### Application Controller

- The `get '/registrations/signup'` route has one responsibility: render the sign-up form view. This view can be found in `app/views/registrations/signup.erb`. Notice we have separate view sub-folders to correspond to the different controller action groupings.
- The `post '/registrations'` route is responsible for handling the `POST` request that is sent when a user hits 'submit' on the sign-up form. It will contain code that gets the new user's info from the `params` hash, creates a new user, signs them in, and then redirects them somewhere else.
- The `get '/sessions/login'` route is responsible for rendering the login form.
- The `post '/sessions'` route is responsible for receiving the `POST` request that gets sent when a user hits 'submit' on the login form. This route contains code that grabs the user's info from the `params` hash, looks to match that info against the existing entries in the user database, and, if a matching entry is found, signs the user in.
- The `get '/sessions/logout'` route is responsible for logging the user out by clearing the `session` hash.
- The `get '/users/home'` route is responsible for rendering the user's homepage view.

#### The `models` Folder

The `models` folder is pretty straightforward. It contains one file because we only have one model in this app: `User`.

The code in `app/models/user.rb` will be pretty basic. We'll validate some of the attributes of our user by writing code that makes sure no one can sign up without inputting their name, email, and password. More on this later.

#### The `views` Folder

This folder has a few sub-folders we want to take a look at. Since we have different controllers responsible for different functions/features, we want our `views` folder structure to match up.

- The **`views/registrations`** sub-directory contains one file, the template for the new user sign-up form. That template will be rendered by the `get '/registrations/signup'` route in our controller. This form will `POST` to the `post '/registrations'` route in our controller.
- The **`views/sessions`** sub-directory contains one file, the template for the login form. This template is rendered by the `get '/sessions/login'` route in the controller. The form on this page sends a `POST` request that is handled by the `post '/sessions'` route.
- The **`views/users`** sub-directory contains one file, the template for the user's homepage. This page is rendered by the `get '/users/home'` route in the controller.
- We also have a `home.erb` file in the top level of the `views` directory. This is the page rendered by the root route, `get '/'`.