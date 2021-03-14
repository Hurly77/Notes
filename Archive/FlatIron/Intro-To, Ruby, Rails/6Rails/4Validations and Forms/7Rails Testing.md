- **Models** (RSpec)
- **Controllers** (RSpec)
- **Features** (RSpec/Capybara)

### RSpec

By default, Rails uses `Test::Unit` for testing, which keeps its tests in the mysteriously-named `test/` folder.

If you're planning from the start to use RSpec instead, you can tell Rails to skip `Test::Unit` by passing the `-T` flag to `rails new`, like so:

```
rails new cool_app -T
```

Then, you will add the gem to your Gemfile:

```
gem 'rspec-rails'
```

And use the built-in generator to add a `spec` folder with the right boilerplate:

```ruby
bundle install
bundle exec rails g rspec:install
```

This is the Rails equivalent of the usual `rspec --init`.

Suppose we're working with this model:

```ruby
# app/models/monster.rb
 
class Monster < ActiveRecord::Base
  validates :name, presence: true
  validates :size, inclusion: { in: ["tiny", "average", "like, REALLY big"] }
  validates :taxonomy, format: { with: /\A[A-Z](\.|[a-z]+) [a-z]{2,}\z/,
    message: "must include genus and species, like 'Homo sapiens'" }
end
```

## Testing for Validity

First, we'll make sure that it understands a valid Monster:

```ruby
# spec/models/monster_spec.rb
 
describe Monster do
  let(:attributes) do
    {
      name: "Dustwing",
      size: "tiny",
      taxonomy: "Abradacus nonexistus"
    }
  end
 
  it "is considered valid" do
    expect(Monster.new(attributes)).to be_valid
  end
end
```

#### What is `let`?

If you haven't seen `let` before, it is a [standard helper method](http://www.relishapp.com/rspec/rspec-core/docs/helper-methods/let-and-let) that takes a symbol and a block. It runs the block **once per example** in which it is called and saves the return value in a local variable named according to the symbol. This means you get a fresh copy in every test case.

#### Why is `let` better than `before :each`?

It's more fine-grained, which means you have better **control over your data**. It can be used in combination with `before` statements to set up your test data *just right* before the examples are run.

##### Why did we use `let` to make an attribute hash?

We could have put the entire `Monster.new` call inside our `let` block, but using an attribute hash instead has some advantages:

- If we want to tweak the data first, we can just pass `attributes.merge(name: "Other")` while preserving the rest of the attributes.
- We can also refer to `attributes` when making assertions about what the actual object should look like.

It's a good balance between saving keystrokes and maintaining the flexibility of your test data.

##### Where does `be_valid` come from?

This code uses a neat trick that RSpec refers to as "[predicate matchers](https://www.relishapp.com/rspec/rspec-expectations/docs/built-in-matchers/predicate-matchers)", and you'll see it **a lot** in Rails testing.

In Ruby, **it's conventional** for methods that return `true` or `false` to be named with a question mark at the end.

**"predicate"** is an English grammar term for the part of a sentence that **makes** a statement about the **subject**.

In RSpec, when you call a nonexistent matcher (such as `be_valid`), it strips off the `be_` (`valid`), adds a question mark (`valid?`), and checks to see if the object responds to a method by that name (`monster.valid?`).

### Testing for Validation Failure

```ruby
# spec/models/monster_spec.rb
 
  let(:missing_name) { attributes.except(:name) }
  let(:invalid_size) { attributes.merge(size: "not that big") }
  let(:missing_species) { attributes.merge(taxonomy: "Abradacus") }
 
  it "is invalid without a name" do
    expect(Monster.new(missing_name)).not_to be_valid
  end
 
  it "is invalid with an unusual size" do
    expect(Monster.new(invalid_size)).not_to be_valid
  end
 
  it "is invalid with a missing species" do
    expect(Monster.new(missing_species)).not_to be_valid
  end
```

**Note** that each of these `let` blocks rely on the first one, `attributes`, which contains all of our valid attributes. `missing_name` uses the Rails hash helper `except` to exclude the `name` key while the other two use the standard Ruby `merge` method to overwrite valid attributes with invalid ones.

## Any other RSpec features to know about?

Several RSpec features have been moved over time into the [rspec-collection_matchers](https://github.com/rspec/rspec-collection_matchers) gem, which can make some detailed assertions more readable.

### Controller Tests

```ruby
# spec/controllers/monsters_controller_spec.rb
 
describe MonstersController, type: :controller do
  let(:attributes) do
    {
      name: "Dustwing",
      size: "tiny",
      taxonomy: "Abradacus nonexistus"
    }
  end
 
  it "renders the show template" do
    monster = Monster.create!(attributes)
    get :show, id: monster.id
    expect(response).to render_template(:show)
  end
 
  describe "creation" do
    before { post :create, monster: attributes }
    let(:monster) { Monster.find_by(name: "Dustwing") }
 
    it "creates a new monster" do
      expect(monster).to_not be_nil
    end
 
    it "redirects to the monster's show page" do
      expect(response).to redirect_to(monster_path(monster))
    end
  end
end
```

### Feature Tests

This is called a **unit test**, because it tests a single unit of functionality.

**acceptance test** - This test relies on the view (steering wheel), the model (tires), *and* the controller (steering column)(**It could** also be called an **integration test** because it tests more than one piece of the system at once.)

The **acceptance test** at the top of this section covers too much ground, making it brittle and difficult to maintain.

The **unit test** in the middle of this section is so specific that it almost feels like we just rewrote the controller code with different phrasing.

The **feature test**, on the other hand, is Just Right. It lets us think like a user (in terms of the steering wheel, or view) while still making intelligent assertions about how the underlying system should respond to input (in terms of the steering column, or controller).

Now, on to the *how*.

## Capybara

When you see key words like `visit`, `fill_in`, and `page`, you know you're looking at a [Capybara](https://github.com/jnicklas/capybara) test.

To set up Capybara, one must first add the gem to the `Gemfile`:

```ruby
gem 'capybara'
```

Then set up Capybara-Rails integration in `spec/rails_helper.rb`:

```ruby
require 'capybara/rails'
```

Then set up Capybara-RSpec integration in `spec/spec_helper.rb`:

```ruby
require 'capybara/rspec'
```

Feature tests are traditionally located in `spec/features`, but you can put them anywhere if you pass the `:type => :feature` option to your `describe` call.

To test our monster manager with Capybara, we'll start by setting up a `GET` request and then use Capybara's convenient helper functions to interact with the page just like a user would:

```ruby
# spec/features/monster_creation.rb
 
describe "monster creation", type: :feature do
  before do
    visit new_monster_path
    fill_in "Name", with: "Dustwing"
    select "tiny", from: "monster_size"
    fill_in "Taxonomy", with: "Abradacus nonexistus"
    click_button "Create Monster"
  end
```

Now, we can write our original controller tests like usual:

```ruby
et(:monster) { Monster.find_by(name: "Dustwing") }
 
  it "creates a monster" do
    expect(monster).to_not be_nil
  end
 
  it "redirects to the new monster's page" do
    expect(current_path).to eq(monster_path(monster))
  end
```

And because we're in Capybara land, we also have a very convenient way of making assertions about the final `GET` request:

```ruby
  it "displays the monster's name" do
    within "h1" do
      expect(page).to have_content(monster.name)
    end
  end
```

`within` sets the context for our next expectation, restricting it to the first `<h1>` tag encountered on the page. This way, our `expect` call will only pass if the specified content (`"Dustwing"`) appears inside that first headin

These can serve as fairly reliable guidelines:

- Models should always be thoroughly unit tested.
- Controllers should be as thin as possible to keep your feature tests simple.
- If you can't avoid making a controller complex, it deserves its own isolated test.
- Capybara's syntax is much more powerful than Rails's built-in functionality for view tests, so stick with it whenever possible.
- Don't get carried away with the details when testing views: you just need to make sure the information is in the right place. If your tests are too strict, it will be impossible to make even simple tweaks to your templates without breaking the build.