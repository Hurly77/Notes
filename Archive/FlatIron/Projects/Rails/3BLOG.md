How to set up devise and omniauth-facebook 

Unfortunately, I spent pretty much the first week trying to set up my omniauth,  I know how sad that sounds, as frustrating as it was, it lead me to devise. Now if your anything like me and any you need something a little more visual to help you out, follow this [link](https://www.youtube.com/watch?v=Dd8dOAL6WYs&t=266s) it will take you to the walk though, that delivered me from this nightmare.

Now, I think the best strategy is to go the respective sight that you'll be using and then register as a developer and, create your ID, SECRET, key. 

then you'll want to install the gem both `omniauth` and`omniauth-facebook` and of course use `bundle install ` to install the gems and their dependencies. You'll also be using a gem called `devise` go figure, right and `activerecord-session_store`

You'll then want to head over to you console an run the command `EDITOR="your_editor --wait" rails credentials:edit` this will bring up you credentials with is mainly binary data . Then input your, ID and KEY like so:

```
facebook:
  facebook_client_id: 12314354365546
  facebook_client_secret: 465476657657
```

Also, you'll want to got to, [devise](https://github.com/heartcombo/devise) to look up docs on how to user views and controller and so on. Use `rails generate devise:install` to get the devise.rb in you initializers.  

```ruby
config.omniauth :facebook, Rails.application.credentials.dig(:facebook, :facebook_client_id),
   Rails.application.credentials.dig(:facebook, :facebook_client_secret), scope: 'public_profile,email'
```

There is a lot going on here but from context you can mostly derive what this code is doing. next move on to creating a `sessions_store.rb ` in your initializers folder 

```ruby
Rails.application.config.session_store :active_record_store, key: '_devise-omniauth_session'
```

and a sessions migration in you db

```ruby
  def change
    create_table :sessions do |t|
      t.string :session_id, null: false
      t.text :data
    end
    add_index :sessions, :session_id, unique: true
    add_index :sessions, :update_at
  end
```

if you haven't already you'll also want to add attributes for `uid` and `provider`. 

Then Create a User class to inherit from application and make a meth to that will create a user from a provider like so

```ruby
class User < ApplicationRecord
    devise :database_authenticatable, :registerable,
          :recoverable, :validatable,
          :omniauthable, omniauth_providers: [:facebook]

  def self.create_from_provider_data(provider_data)
    where(provider: provider_data.provider, uid: provider_data.uid).first_or_create do |user|
      user.name = provider_data.info.name
      user.email = provider_data.info.email
      user.password = Devise.friendly_token[0, 20]
    end
  end
end
```

It  isn't necessary to put all those devise attributes but some of them come in pretty handy.

next move onto you OmniauthController, here you'll be useing the method you made in your  user class.

```ruby
class OmniauthController < ApplicationController
  def facebook
    @user = User.create_from_provider_data(request.env['omniauth.auth'])
  
    if @user.persisted?
      sign_in_and_redirect @user
    else
      flash[:error] = 'There was a problem'
      redirect_to new_user_registration_url
    end
  end

    def failure
      flash[:error] = 'There was a problem signing you in. Please register or try signing in later'
      redirect_to new_user_registration_url
    end
end
```

and that's it, you should already have a devise folder in you views and if you don't check [documentation](https://github.com/heartcombo/devise) to see how or for any father questions you might have

