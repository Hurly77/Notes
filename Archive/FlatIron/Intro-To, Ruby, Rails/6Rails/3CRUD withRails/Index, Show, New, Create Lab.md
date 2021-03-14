##### config/secrets.yml

YAML stands for **Y**AML **A**in't **M**arkup **L**anguage

 **is a human-readable data-serialization language**. It is commonly used for configuration *files* and in applications where data is being stored or transmitted.

##### secrets.yml

```yml
# Be sure to restart your server when you modify this file.

# Your secret key is used for verifying the integrity of signed cookies.
# If you change this key, all old signed cookies will become invalid!

# Make sure the secret is at least 30 characters and all random,
# no regular words or you'll be exposed to dictionary attacks.
# You can use `rake secret` to generate a secure secret key.

# Make sure the secrets in this file are kept private
# if you're sharing your code publicly.

development:
  secret_key_base: c05fe83eb4cd2f4f87dcb280f89c4336a1c93deca0d01ec0e535c63fdf762b74e6d31235323665d766211de551b1e7ed2c8ec8073fdbcdac8b9f1bb645ebf943

test:
  secret_key_base: 3dd314c3109ff7c4047b551070fd030c7c4ef19175c7f51c871539406385c72387d6710b088e3112f601f0a1e9ef59635a6d8d584a2df41b2941ff05d76a53bf

# Do not keep production secrets in the repository,
# instead read values from the environment.
production:
  secret_key_base: <%= ENV["SECRET_KEY_BASE"] %>

```

##### routes

```ruby
Rails.application.routes.draw do
resources :coupons, only: [:index, :new, :create]
  get "/coupons/:id", to: "coupons#show", as: 'coupon'
end
# The priority is based upon order of creation: first created -> highest priority.
  # See how all your routes lay out with "rake routes".

  # You can have the root of your site routed with "root"
  # root 'welcome#index'

  # Example of regular route:
  #   get 'products/:id' => 'catalog#view'

  # Example of named route that can be invoked with purchase_url(id: product.id)
  #   get 'products/:id/purchase' => 'catalog#purchase', as: :purchase

  # Example resource route (maps HTTP verbs to controller actions automatically):
  #   resources :products

  # Example resource route with options:
  #   resources :products do
  #     member do
  #       get 'short'
  #       post 'toggle'
  #     end
  #
  #     collection do
  #       get 'sold'
  #     end
  #   end

  # Example resource route with sub-resources:
  #   resources :products do
  #     resources :comments, :sales
  #     resource :seller
  #   end

  # Example resource route with more complex sub-resources:
  #   resources :products do
  #     resources :comments
  #     resources :sales do
  #       get 'recent', on: :collection
  #     end
  #   end

  # Example resource route with concerns:
  #   concern :toggleable do
  #     post 'toggle'
  #   end
  #   resources :posts, concerns: :toggleable
  #   resources :photos, concerns: :toggleable

  # Example resource route within a namespace:
  #   namespace :admin do
  #     # Directs /admin/products/* to Admin::ProductsController
  #     # (app/controllers/admin/products_controller.rb)
  #     resources :products
  #   end
```

```ruby
class CreateCoupon < ActiveRecord::Migration
  def change
    create_table :coupons do |t|
      t.string :coupon_code
      t.string :store

      t.timestamps
    end
  end
end
```



```ruby
class Coupon < ActiveRecord::Base
  def to_s
    self.coupon_code + " " + self.store 
      #forgot to pust self ran into a few problems here
  end
end
```

```ruby
class CouponsController < ApplicationController

  def index
    @coupons = Coupon.all    
  end

  def show
    @coupon = Coupon.find(params[:id])
  end
  
  def new
  end

  def create
    @coupon = Coupon.new
    @coupon.coupon_code = params[:coupon][:coupon_code]
    @coupon.store = params[:coupon][:store]
      #test wanted me to nest the coupon into a key :coupon
    @coupon.save
    redirect_to coupon_path(@coupon)
      #there is a singluar method coupon and a plural method coupons
  end
end
```



##### app/views/coupons/index.rb

```erb
<div>
  <% @coupons.each do |coupon| %>
   <div><%= link_to coupon.to_s, coupon_path(coupon) %></div>
    <!--Same thing here you have to use singular form of _path for show-->
  <% end %>
</div>

```

##### app/views/coupons/new.rb

```erb
<h1>Coupon Form</h1>
<%= form_tag coupons_path do %>
<label>Coupon Code</label><br>
<%= text_field_tag :'coupon[coupon_code]' %><br>

<label>Store</label><br>
<%= text_field_tag :'coupon[store]' %><br>

<%= submit_tag "Submit Coupon" %>
<% end %>
<%= params.inspect %>
```

##### app/views/coupons/show.rb

```erb
<h1><%= @coupon.store %></h1> 
<h1><%= @coupon.coupon_code%></h1>
```

