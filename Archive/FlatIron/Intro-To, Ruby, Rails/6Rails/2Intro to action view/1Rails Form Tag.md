## Rails Forms

#### specs

follow the **TDD** process(**T**est **D**riven **D**evelopment)

```ruby
#spec/features/post_spec.rb
require 'rails_helper'
 
describe 'new post' do
  it 'ensures that the form route works with the /new action' do
    visit new_post_path
    expect(page.status_code).to eq(200)
  end

  it 'renders HTML in the /new template' do
    visit new_post_path
    expect(page).to have_content('Post Form')
  end

  it "displays a new post form that redirects to the index page, which then contains the submitted post's title and description" do
    visit new_post_path
    fill_in 'post_title', with: 'My post title'
    fill_in 'post_description', with: 'My post description'
 
    click_on 'Submit Post'
 
    expect(page.current_path).to eq(posts_path)
    expect(page).to have_content('My post title')
    expect(page).to have_content('My post description')
  end
end
```

### Steps to start form

```ruby
# config/routes.rb
#first we need a route for new so we add new to only:
resources :posts, only: [:index, :new, :create]
#then we need a action method for our new route
#later we'll have to build a create route for our create action in out controller
#app/controller/student_controller.rb
def new
end

def create
    #later in the lab we had to create an object
      Post.create(title: params[:post][:title], description: params[:post][:description])
  redirect_to posts_path #this is rails rederict method
end
```

#### The form view before we use form_tag

06/15/2020

**Note:** For the next few labs, we're not going to use mass assignment; instead we'll assign each attribute individually. For example, instead of `Student.create(params[:students]) we'll write Student.create(first_name: params[:first_name], last_name: params[:last_name])` and name our fields in the view files without the "student" preface. We'll discuss why in the upcoming reading on Strong Params.

```erb
<form>
  <label>Post title:</label><br>
  <input type="text" id="post_title" name="post[title]"><br>
 
  <label>Post description:</label><br>
  <textarea id="post_description" name="post[description]"></textarea><br>
 
  <input type="submit" value="Submit Post">
</form>
 
<%= params.inspect %>
```

Traditionally, Rails apps use that `model[attribute]` syntax for `name` attributes (e.g., `post[title]`).

#### steps

```erb
<form action="<%= posts_path %>">
    <!---here we need a action attribute-->
    <!--but we also needed a method attribute so that app know where we are submitting form data cia the POST HTTP verb-->
    <form action="<%= posts_path %>" method="POST">
```

Then we run into a problem **ActionController::InvalidAuthenticityToken **Which leads us to a very important part of **Rails forms: CSRF.**

**Note:** If you are seeing an error along the lines of `Cannot render console from (<IP address here>)! Allowed networks: 127.0.0.1, ::1, 127.0.0.0/127.255.255.255` you'll want to add this code to `config/environments/development.rb`, and not `config/application.rb`, so it is only applied in your development environment. `config.web_console.whitelisted_ips = '<IP address here>'`

**What is CSRF?** 

**"CSRF" stands for: Cross-Site Request Forgery. **let's walk through a real-life example of a Cross-Site Request Forgery

1. You go to your bank website and log in. After checking your balance, you open up a new tab in the browser and go to your favorite meme site.
2. Unbeknownst to you, the meme site is actually a hacking site that has scripts running in the background as soon as you land on their page.
3. One of the scripts on the site hijacks the banking session that's open in the other browser tab and submits a form request to transfer money to their account.
4. The banking form can't tell that the form request wasn't made by you, so it goes through the process as if you were the one who made the request

**Rails blocks this from happening by default by requiring that a unique authenticity token be submitted with each form.**

```ruby
class Application < Rails::Application
  config.web_console.whitelisted_ips = '<IP address here>'
end
```

we can integrate the `form_authenticity_token` helper into the form as a hidden field:

```erb
 <input type="hidden" name="authenticity_token" value="<%= form_authenticity_token %>">
```

**ActionView**  a sub-gem of Rails, provides a number of helper methods to assist with streamlining view template code.

 integrating a Rails `form_tag` element:

```erb
<%= form_tag posts_path do %>
<% end %>
<%= hidden_field_tag :authenticity_token, form_authenticity_token %>
```

The `form_tag` Rails helper is **smart enough to know that we want to submit the form via the** `POST` method, and it automatically **renders the HTML** that we were writing by hand before. The `form_tag` method **also automatically generates the necessary authenticity token**, so we can remove the now-redundant `hidden_field_tag`.

`text_field_tag` and a `text_area_tag` and passing each the corresponding `name` attribute as a symbol. It's **important to keep in mind** that form helpers **aren't magic** –– they're **simply Ruby methods that accept arguments**, such as the `name` attribute and any additional parameters related to the form's elements

##### After form_tag

```erb
<%= form_tag posts_path do %>
  <label>Post title:</label><br>
  <%= text_field_tag :'post[title]' %><br>
 
  <label>Post description:</label><br>
  <%= text_area_tag :'post[description]' %><br>
 
  <%= submit_tag "Submit Post" %>
<% end %>
```





