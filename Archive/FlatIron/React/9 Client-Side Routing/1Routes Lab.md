```jsx
//index.js

import React from 'react';
import { render } from 'react-dom';
import App from './containers/App';

render (
  <App />,
  document.getElementById('root')
);

```

```jsx
//containers/App.js

import React from 'react';
import {
  BrowserRouter as Router,
  Route
} from 'react-router-dom';
import NavBar from '../components/NavBar';
import Home from '../components/Home';
import Actors from '../components/Actors';
import Directors from '../components/Directors';
import Movies from '../components/Movies';


const App = (props) => {
  return (
    <Router>
      <div>
      <NavBar /> 
      <Route exact path="/" component={Home} />
      <Route exact path="/actors" component={Actors} />
      <Route exact path="/directors" component={Directors} />
      <Route exact path="/movies" component={Movies} />
      </div>
    </Router>
  );
};

export default App
```

```jsx
//components/Actors.js

import React from 'react';
import { actors } from '../data';

const Actors = () => {
  return (
    <div>
      <h1>Actors Page</h1>
      {actors.map((actor, id)=>{
        return(
          <div key={id}>
            <h2>{actor.name}</h2>
            <ul>
              {actor.movies.map((movie, id)=>{
               return( <li key={id}>{movie}</li>)
              })}
            </ul>
          </div>
        )
      })}
    </div>
  );
};

```

```jsx
//components/Directors.js

import React from 'react';
import { directors } from '../data';

const Directors = () => {
  return (
    <div>
      <h1>Directors Page</h1>
      {directors.map((director, id)=>{
        return(
        <div key={id}>
          <h2>{director.name}</h2>
          <ul>
          {director.movies.map((movie, id)=>{
            return <li key={id}>{movie}</li>
          })}
          </ul>
        </div>)
      })}
    </div>
  );
}

export default Directors
```

```jsx
//components/Home.js

import React from 'react';

const Home = () => {
  return (
    <div>
      <h1>Home Page</h1>
    </div>
  );
};

export default Home;
```

```jsx
//components/Movies.js

import React from 'react';
import { movies } from '../data';

const Movies = () => {
  return (
    <div>
      <h1>Movies Page</h1>
        {movies.map((movie, id)=>{
         return ( 
          <div key={id}>
            <h2>{movie.title}</h2>
            <ul>
              <li>{movie.time}</li>
            </ul>
            <ul>
            {movie.genres.map((genre, id)=>{
              return (<li key={id}>{genre}</li>)
            })}
            </ul>            
          </div>)
        })}
    </div>
  );
};

export default Movies;
```

```jsx
//components/NaveBar.js

import React from 'react';
import { NavLink } from 'react-router-dom';

const NavBar = () => {
  return (
    <div className="navbar">
      <NavLink to="/" exact >Home</NavLink>
      <NavLink to="/movies" exact >Movies</NavLink>
      <NavLink to="/directors" exact >Directors</NavLink>
      <NavLink to="/actors" exact >Actors</NavLink>
    </div>
  );
};

export default NavBar;
```

