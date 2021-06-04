An **API**, short for (**A**pplication **P**rogramming **I**nterface) - **putting it broadly**, is a way for one **system** to **communicate** with other ***external*** **systems**. Using Rails, *we can create our own APIs from scratch*, 

Rails APIs render JSON strings, which is particularly useful, since we can use them when building frontends using JavaScript, DOM manipulation and asynchronous requests.

## Review of MVC Structure

The model, view, controller structure is a separation of concerns where groups of files have specific jobs and interact with each other in very defined ways:

- **Models:** The 'logic' of a web application. This is where data is manipulated and/or saved to a database.
- **Views:** The 'frontend', user-facing part of a web application - this is the only part of the app that the user interacts with directly. Views generally consist of HTML, CSS, and Javascript.
- **Controllers:** The go-between for models and views. The controller relays data from the browser to the application, and from the application to the browser.

**ASIDE:** By inheriting from `ApplicationRecord`, `Bird` also inherits from [`ActiveRecord`](https://guides.rubyonrails.org/active_record_basics.html), which you may remember is an [ORM](https://en.wikipedia.org/wiki/Object-relational_mapping), or Object Relational Map. Because of this, we gain many useful methods like `all` and `save` without having to include any additional methods.

Rails favors convention over configuration. For this reason, if a ***folder\*** and ***file\*** are present in the ***views\*** folder that correspond to a ***controller\*** and ***action\*** listed on a ***route\***, Rails will display that ***view\*** by default.

# Render Multi-Content types : Rails

To render plain text from a Rails controller, you specify `plain:`, followed by the text you want to display:

```ruby
class BirdsController < ApplicationController
  def index
    @birds = Bird.all
    render plain: "Hello #{@birds[3].name}"
  end
end
#In the browser, this displays as:
#=> Hello Mourning Dove
```

open the browser console and run the following:

```js
fetch('http://localhost:3000/birds').then(response => response.text()).then(text => console.log(text))
```

**Notice**: On resolution of the `fetch()` request here, in the first `.then()`, `response.text()` is called, since we're handling plain text.

### Render JSON From a Controller

To render *JSON* from a Rails controller, you specify `json:` followed by data that can be converted to valid JSON:

```ruby
class BirdsController < ApplicationController
  def index
    @birds = Bird.all
    render json: 'Remember that JSON is just object notation converted to string data, so strings also work here'
  end
end
```

We can pass strings as we see above, as well as hashes, arrays, and other data types:

```ruby
class BirdsController < ApplicationController
  def index
    @birds = Bird.all
    render json: { message: 'Hashes of data will get converted to JSON' }
  end
end
__________________________________________________________
class BirdsController < ApplicationController
  def index
    @birds = Bird.all
    render json: ['As','well','as','arrays']
  end
end
```

In our bird watching case, we actually already have a collection of data, `@birds`, so we can write:

```ruby
class BirdsController < ApplicationController
  def index
    @birds = Bird.all
    render json: @birds
  end
end
```

With the Rails server running, check out `http://localhost:3000/birds`. You should see that Rails has output all the data available from all the `Bird` records

```ruby
[
  {
      "id": 1,
      "name": "Black-Capped Chickadee",
      "species": "Poecile Atricapillus",
      "created_at": "2019-05-09T11:07:58.188Z",
      "updated_at": "2019-05-09T11:07:58.188Z"
    },
    {
      "id": 2,
      "name": "Grackle",
      "species": "Quiscalus Quiscula",
      "created_at": "2019-05-09T11:07:58.195Z",
      "updated_at": "2019-05-09T11:07:58.195Z"
    },
    {
      "id": 3,
      "name": "Common Starling",
      "species": "Sturnus Vulgaris",
      "created_at": "2019-05-09T11:07:58.199Z",
      "updated_at": "2019-05-09T11:07:58.199Z"
    },
    {
      "id": 4,
      "name": "Mourning Dove",
      "species": "Zenaida Macroura",
      "created_at": "2019-05-09T11:07:58.205Z",
      "updated_at": "2019-05-09T11:07:58.205Z"
    }
]
```

we could send another `fetch()` request to the same place, only this time, since we're handling JSON, we'll swap out `text()` for `json()`:

```json
fetch('http://localhost:3000/birds').then(response => response.json()).then(object => console.log(object))
// > Promise {<pending>}
 > [{…}, {…}, {…}, {…}]
 
// 0: {id: 1, name: "Black-Capped Chickadee", species: "Poecile Atricapillus", created_at: "2019-05-09T11:07:58.188Z", updated_at: "2019-05-09T11:07:58.188Z"}
// 1: {id: 2, name: "Grackle", species: "Quiscalus Quiscula", created_at: "2019-05-09T11:07:58.195Z", updated_at: "2019-05-09T11:07:58.195Z"}
// 2: {id: 3, name: "Common Starling", species: "Sturnus Vulgaris", created_at: "2019-05-09T11:07:58.199Z", updated_at: "2019-05-09T11:07:58.199Z"}
// 3: {id: 4, name: "Mourning Dove", species: "Zenaida Macroura", created_at: "2019-05-09T11:07:58.205Z", updated_at: "2019-05-09T11:07:58.205Z"}
```

You may also often see more detailed nesting in the `render json:` statement:

```ruby
class BirdsController < ApplicationController
  def index
    @birds = Bird.all
    render json: { birds: @birds, messages: ['Hello Birds', 'Goodbye Birds'] }
  end
end
```

**Rather than an array** of four birds, an object with two keys, each pointing to an array would be rendered instead

##### NOTE:

***Well structured API data can make frontend code simpler. Poorly structured API data can lead to complicated nests of JavaScript enumerable.***

We can choose to explicitly convert our array or hash, without any problem by adding `to_json` to the end:

```RUBY
class BirdsController < ApplicationController
  def index
    @birds = Bird.all
    render json: { birds: @birds, messages: ['Hello Birds', 'Goodbye Birds'] }.to_json
  end
end
```

This will produce the same result as it did before. This will produce the same result as it did before.

Rails  'magic' to handle things like this and keep us from having to write `to_json` on the same line as `render json:` in every action we write.

## Say Goodbye to Instance Variables

```RUBY
class BirdsController < ApplicationController
  def index
    @birds = Bird.all
    render json: @birds
  end
end
```

we no longer need to deal with instance variables and can instead just use a local variable:

```RUBY
class BirdsController < ApplicationController
  def index
    birds = Bird.all
    render json: birds
  end
end
```

This is how we will be displaying our examples going forward.