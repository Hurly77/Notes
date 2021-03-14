**REST** (REpresentational State Transfer)

Here is a mapping of all of the different route helpers, HTTP verbs, paths, and controller action mappings for our newsletter feature.

| Method | Action                | Description                                   |
| ------ | --------------------- | --------------------------------------------- |
| GET    | /newsletters          | Show all newsletters                          |
| POST   | /newsletters          | Create a new newsletter                       |
| GET    | /newsletters/new      | Render the form for creating a new newsletter |
| GET    | /newsletters/:id/edit | Render the form for editing a newsletter      |
| GET    | /newsletters/:id      | Show a single newsletter                      |
| PATCH  | /newsletters/:id      | Update a newsletter                           |
| DELETE | /newsletters/:id      | Delete a newsletter                           |

Below is a breakdown of each of the controller actions and what it represents. Notice the direct correlation between the route mapping above and the controller methods:

| Method  | Description                                   |
| ------- | --------------------------------------------- |
| index   | Show all newsletters                          |
| create  | Create a new newsletter                       |
| new     | Render the form for creating a new newsletter |
| edit    | Render the form for editing a newsletter      |
| show    | Show a single newsletter                      |
| update  | Update a newsletter                           |
| destroy | Delete a newsletter                           |

Here is a diagram that shows how the views, controller actions, routes, and HTTP verbs are all mapped together:

![REST Diagram](https://curriculum-content.s3.amazonaws.com/web-development/rails-intro-to-rest/rails_routes.png)

## Definition of HTTP Verbs

So what do `GET`, `POST`, et al. represent? Those are HTTP verbs that each give the HTTP request unique behavior. Below is an explanation of each verb:

- **GET** - The GET method retrieves whatever information is identified by the Request URI. This means if you go to `/posts`, you will get all of the posts that the application wants your application to have.
- **POST** - The POST method is used to send data enclosed in the request to the server. The server is expected to use this data to create some new resource.
- **PATCH/PUT** - The PUT/PATCH methods both represent the HTTP verbs that are used to update existing resources. So if you sent a `PUT` request to `/posts/1` with a new post name, the post with an `id` of 1 would be updated.
- **DELETE** - The DELETE method requests that the server delete the resource identified by the Request URI. This means… that it deletes the record. It's nice and explicit.

You can **(and should!)** read more about Rails routing [here](http://guides.rubyonrails.org/routing.html).