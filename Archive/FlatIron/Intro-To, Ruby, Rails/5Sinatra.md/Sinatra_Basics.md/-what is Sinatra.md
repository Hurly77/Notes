## What's a Web Framework?

A web application framework (WAF)  is a software framework that is designed to support the development of dynamic websites web applications, web services and web resources. The framework aims to alleviate the overhead associated with common activities performed in web development

Frameworks provide structure and libraries that allow you to focus on **your** application and not **applications in general**. The bigger the framework, the more you can rely on it to provide you with common needs. The smaller the framework, the more you'll have to build things yourself.

### What is Sinatra?

Sinatra is a Domain Specific Language implemented in Ruby that's used for writing web applications. **Essentially**, Sinatra is nothing more than some **pre-written methods** that we can include in our applications to turn them into Ruby web applications.

## The Sinatra DSL

**D**omain **S**pecific **L**anguage

Any application that requires the `sinatra` library will get access to methods like: `get` and `post`. These methods provide the ability to instantly transform a Ruby application into an application that can respond to HTTP requests.

### Basic Sinatra Applications

First, make sure Sinatra is installed by running `gem install sinatra` in your terminal.

The simplest Sinatra application would be:

File: `app.rb`

```ruby
require 'sinatra' class App < Sinatra::Base   get '/' do    "Hello, World!"  end end
```

You could start this web application by running `rackup app.rb`. You'll see something similar to:

```
[2019-09-23 13:29:58] INFO  WEBrick 1.4.2
[2019-09-23 13:29:58] INFO  ruby 2.6.1 (2019-01-30) [x86_64-darwin17]
[2019-09-23 13:29:58] INFO  WEBrick::HTTPServer#start: pid=82970 port=9292
```

```ruby

```

