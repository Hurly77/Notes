### Keeping Code Organized

**Keeping our code organized** is crucial when developing complex applications. This concept is called `separation of concerns` and `single responsibility`

##### What does a Sinatra MVC File Structure Look Like?

```
├── Gemfile
├── README.md
├── app
│   ├── controllers
│   │   └── application_controller.rb
│   ├── models
│   │   └── model.rb
│   └── views
│       └── index.erb
├── config
│   └── environment.rb
├── config.ru
├── public
│   └── stylesheets
└── spec
    ├── controllers
    ├── features
    ├── models
    └── spec_helper.rb
```

#### `Gemfile`

This holds a list of all the gems needed to run the application. This give us our **bundle install** command: It will create a `Gemfile.lock` file for you, hich **is just a documentation of the specific gem versions that should be installed**



#### `app` directory

This folder holds our MVC directories - `models`, `views`, and `controllers`. We spend most of our time coding in this directory.



#### `models` directory

This directory holds the **logic** behind our application. These files **represent** a **User**, **Post**, or **Comment**, or a unit of **work**.  Each file in models typically contains a different class. For example, `dog.rb` would contain a class called `Dog`



#### `controllers` directory

The **controllers**, such as `application_controller.rb`, are where the application configurations, **routes**, and **controller actions** are implemented. Sometimes our other controllers will use `ApplicationController` as an **inheritance** point so that they inherit all the defaults and behaviors defined **in** our main `ApplicationController`. Other times our other controllers will simply inherit from `Sinatra::Base`. Controllers represent the **application logic**, generally; the interface and flow of our application.



#### `views` directory

This directory holds the code that will be displayed in the browser. In a Sinatra app we use `.erb` files instead of `.html` files because .**erb** **files** **allow** us to include regular, old **HTML** tags AND **special** erb tags which contain **Ruby** code. 



#### `config.ru` file

A `config.ru` file is necessary when building Rack-based applications and using `rackup`/`shotgun` to start the application server (the ru stands for rackup). `config.ru` then specifies which controllers to load as part of our application using `run` or `use`. run ApplicationController



#### `config` directory

This directory holds an `environment.rb` file. We'll be using this file to connect up all the files in our application to the appropriate gems and to each other.



#### `public` directory

The `public` directory holds our front-end assets. 



#### `spec` directory

The `spec` directory contains any tests for our applications. These tests set up any expectations for the rest of the project.