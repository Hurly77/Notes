## 

**“Master-Detail” interface** - I some piece of interface that provides access to the entire resource which we can use to select specific items from.

## Nesting

Think of YouTube again for a moment. Let's pretend that visiting `/videos` displays a list of videos. Clicking on any video should keep our list of videos on the page, but also display details on the selected video. This should be updated in the URL - the URL should change to `/videos/:videoId`, where `:videoId` is a unique value (this is slightly different than how YouTube works but the concepts are similar). Using nested **React-Router**, we can write our application so one component, the `List` (of videos) renders using a `Route` that matches the path `/videos`. Then, within `List`, we add a *second* `Route` that renders `Item` when the path matches `/videos/:videoId`.

#### Rendering Our List

**Starter Code**

```jsx
 state = {
    movies: {
      1: { id: 1, title: 'A River Runs Through It' },
      2: { id: 2, title: 'Se7en' },
      3: { id: 3, title: 'Inception' }
    }
  }
```

`App` also has `Router` wrapping everything inside the JSX code

```jsx
<Route exact path="/" render={() => <div>Home</div>} />
<Route path='/movies' render={routerProps => <MoviesPage {...routerProps} movies={this.state.movies}/>} />
```

> **Aside**: Notice that instead of the `component` prop, we're using `render`. The `render` prop works very similarly to `component`, but instead of passing an entire component in, we must pass a function that returns JSX. As we see in the example above, this means we can write JSX code directly, having the function return `<div>Home</div>`. Or, we can have the function return a component, like the second `Route` above.

In the second `Route` **when** we render a component through a `Route` with `Render` as prop, **the function accepts an argument**, `RouterProps`. When the path matches the URL, the `Route` will call the function inside `render` and pass in the current information available about the route, including the URL path that caused the `Route` to render 

So, **if the URL path matche**s `/movies`, the function inside `render` is called. The object that is passed in, `routerProps`, gets passed to the `MoviesPage` component as props. Using the spread operator (`{...routerProps}`) will result in the creation of props for each key present inside the `routerProps` object.

  the component, `MoviesPage`, receives props *from* the `Route` that contain information on the route. In addition, we can also pass in other props, as we see with `movies={this.state.movies}`.

`MoviesPage` component, this component is responsible for loading our `MoviesList` component and passing in the movies we received from `App`.

```jsx
// ./src/containers/MoviesPage.js
import React from 'react';
import { Route } from 'react-router-dom';
import MoviesList from '../components/MoviesList';
 
const MoviesPage = ({ movies }) => (
  <div>
    <MoviesList movies={movies} />
  </div>
)
 
export default MoviesPage
```

**create** our `MoviesList` component to render **React Router** `Link`s for each movie:

```jsx
// ./src/components/MoviesList.js
import React from 'react';
import { Link } from 'react-router-dom';
 
const MoviesList = ({ movies }) => {
  const renderMovies = Object.keys(movies).map(movieID =>
    <Link key={movieID} to={`/movies/${movieID}`}>{movies[movieID].title}</Link>
  );
 
  return (
    <div>
      {renderMovies}
    </div>
  );
};
 
export default MoviesList;
```

The `movies` prop has been passed from `App` to `MoviesPage`, then again to `MoviesList`

To iterate over this object, we'll use `Object.keys(movies)` to get an array of keys

dynamic path in `to`:

```jsx
to={`/movies/${movieID}`}
```

### Linking to the Show

**create** our `MovieShow` component

```jsx
// ./src/components/MovieShow.js
import React from 'react';
 
const MovieShow = props => {
 
  return (
    <div>
      <h3>Movies Show Component!</h3>
    </div>
  );
}
 
export default MovieShow;
```

Next, we import `MovieShow` into `MoviesPage` and add a nested route in our `src/containers/MoviesPage.js` file to display the `MovieShow` container if that route matches `/movies/:movieId`.

```jsx
// .src/containers/MoviesPage.js
import React from 'react';
import { Route } from 'react-router-dom';
import MoviesList from '../components/MoviesList';
// import the `MovieShow` component:
import MovieShow from '../components/MovieShow';
 
// Here we add `match` to the arguments so we can access the path information 
// in `routerProps` that is passed from App.js 
const MoviesPage = ({ match, movies }) => (
  <div>
    <MoviesList movies={movies} />
    // We also add a `Route` component that will render `MovieShow`
    // when a movie is selected
    <Route path={`${match.url}/:movieId`} component={MovieShow}/>
  </div>
)
 
export default MoviesPage
```

we've imported `MovieShow` and **added** a `Route` component.

we are **now** inheriting `match` from `this.props`<= **comes** **from** the `routerProps` passed in from `App` **this is** This is a **POJO** (**p**lain **o**ld **J**avascript **o**bject)

**Using** `match`, we can show stuff depending on what the `match.url` returns. We do this **because** we want the `Route` inside `MoviesPage` to match the exact URL that caused `MoviesPage` to render, plus `:movieId`

our `Link`s are each getting a unique path in the `to={...}` attribute, since each `movieID` is different.

```jsx
// ./src/components/MoviesList.js
import React from 'react';
import { Link } from 'react-router-dom';
 
const MoviesList = ({ movies }) => {
  const renderMovies = Object.keys(movies).map(movieID =>
    <Link key={movieID} to={`/movies/${movieID}`}>{movies[movieID].title}</Link>
  );
 
  return (
    <div>
      {renderMovies}
    </div>
  );
};
 
export default MoviesList;
```

the **data** we want to **display** on a particular `MovieShow` page is available in its parent, `MoviesPage`, as props

**For `MovieShow` to display this content, we will need to make our movies collection available within `MovieShow`.**

```jsx
// .src/containers/MoviesPage.js
import React from 'react';
import { Route } from 'react-router-dom';
import MoviesList from '../components/MoviesList';
import MovieShow from '../components/MovieShow';
 
const MoviesPage = ({ match, movies }) => (
  <div>
    <MoviesList movies={movies} />
    // Adding code to pass the movies to the `MovieShow` component
    <Route path={`${match.url}/:movieId`} component={<MovieShow movies={movies} /> }/>
  </div>
)
 
export default MoviesPage
```

If you recall from the earlier `Route`s in `App`, by using a `Route`'s `render` prop, we can **pass in a function that has access to information about the route itself**. We can rewrite the `Route` in `MoviesPage` to do this:

```jsx
// .src/containers/MoviesPage.js
import React from 'react';
import { Route } from 'react-router-dom';
import MoviesList from '../components/MoviesList';
import MovieShow from '../components/MovieShow';
 
const MoviesPage = ({ match, movies }) => (
  <div>
    <MoviesList movies={movies} />
    // Here we replace the `component` prop with the `render` prop so we can pass the 
    // route information to the `MovieShow` component
    <Route path={`${match.url}/:movieId`} render={routerProps => <MovieShow {...routerProps} movies={movies} /> }/>
  </div>
)
 
export default MoviesPage
```

one of the props we receive from the `Route` is `match`, and `match` contains **all the parameters from the URL!** These parameters are stored as key/value pairs in `match.params`. The key corresponds to whatever we named the parameter in our `Route`, so in this case, the parameter will be `movieId`. We can update `MovieShow` to utilize this parameter in conjunction with the `movies` data that was passed down:

```jsx
// .src/components/MovieShow.js
import React from 'react';
 
// Here we add `match` to the arguments so we can access the path information 
// in `routerProps` that is passed from MoviesPage.js 
const MovieShow = ({match, movies}) => {
  return (
    <div>
      // And here we access the `movieId` stored in match.params to render 
      // information about the selected movie
      <h3>{ movies[match.params.movieId].title }</h3>
    </div>
  );
}
 
export default MovieShow;
```

We've succeeded in creating a "Master-Detail" interface - the list of movies is always present when viewing a particular movie's details.

### What Happens If We Only Visit the First Route?

Well, `MoviesPage` renders due to the top-level `/movies` `Route`, but `MoviesPage` will only render `MoviesList`. There is no default `Route`, so we don't see anything. If we want to create a default `Route` here, we can do so using the `match` prop once again:

```jsx
// .src/containers/MoviesPage.js
import React from 'react';
import { Route } from 'react-router-dom';
import MoviesList from '../components/MoviesList';
import MovieShow from '../components/MovieShow';
 
const MoviesPage = ({ match, movies }) => (
  <div>
    <MoviesList movies={movies} />
    // Adding code to show a message to the user to select a movie if they haven't yet
    <Route exact path={match.url} render={() => <h3>Choose a movie from the list above</h3>}/>
    <Route path={`${match.url}/:movieId`} render={routerProps => <MovieShow {...routerProps} movies={movies} /> }/>
  </div>
)
 
export default MoviesPage
```

Now, when we visit `http://localhost:3000/movies`, we see a message that only appears if there is no additional `movieId` at the end of the URL. This is the nested version of a default route. We can't just write `exact path="/"` since these `Route`s will only render inside the `/movies` `Route`.

To briefly review - we are able to nest `Route`s within each other. Using the Router props we receive from the top-level `Route`, we can nest a second `Route` that extends the URL path of the first. We can actually nest `Route`s as many times as we would like, so if we wanted, we could go fully RESTful and create nested `Route`s inside `MovieShow` as well, allowing us to write URL paths that would look something like this:

```
http://localhost:3000/movies/:movieId/edit
```

To get nested `Route`s to work, we need to utilize route information that is stored in the `match` props. `match` contains both the current URL path in `match.url`, as well as any parameters in `match.params`. We define the parameter names in a `Route`'s path by prepending a colon (`:`) to the front of the name. This name will then show up as a key inside `match.params`.

We can use parameters to look up specific data - in this case matching the key of a `movies` object with the URL parameter, `:movieId`, allowed us to display a particular movie's title.

Nesting routes enables us to build single-page applications in React that *behave* like they have many pages. We can also load up and display specific data dynamically.

In the early days of the internet, we would have had to create separate HTML pages ***for each movie in this application\***. Now, with React, we can write abstract components that fill in the data for each 'page' on demand. Very cool!