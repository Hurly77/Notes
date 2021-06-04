- **Static route** - A static route will render a view that does not change. Typically, you will not send parameters to it. Examples would be a site's about or contact pages.

- **Dynamic route** - Dynamic routes are pages that accept parameters and render different content based on those parameters. An example would be a blog's post page that contains a specific article.

  

- **Explicit rendering** - for explicit rendering, Rails lets you dictate which view file you want to have the controller action mapped to.
- **Implicit rendering** - for implicit rendering, Rails follows a standard convention that automatically looks for the view file with the same name as the controller action.

```ruby
get 'about', to: 'static#about'
```

- The HTTP verb - in this case we're using the `get` HTTP verb.
- The path - `'about'` represents the path in the URL bar that the route will be mapped to.
- The controller action - `'static#about'` tells the Rails routing system that this route should be passed through the `static` controller's `about` action. If the term `action` sounds foreign, actions are just Ruby speak for a method in a controller. So in the `StaticController` will be a method called `about` that gets called when a user goes to `/about`.

**EDD** - error driven development