## We need Address

Components are the heart of React's powerful, declarative programming model. React Router is a collection of navigational components that compose declaratively with your application. Whether you want to have bookmarkable URLs for your web app or a composable way to navigate in React Native, React Router works wherever React is rendering--so take your pick!

# Code Along

## Setting up our Main Routes

With React Router our core routing will live in this component. We will define our various routes within this file. To start using routes, we need to **install** `react-router-dom`:

```
npm install react-router-dom
```

To start implementing routes, we first need to import `BrowserRouter`

```jsx
// .src/index.js
 
import React from 'react';
import ReactDOM from 'react-dom';
// Step 1. Import react-router functions
import { BrowserRouter as Router, Route } from 'react-router-dom';
 
const Home = () => {
  return (
    <div>
      <h1>Home!</h1>
    </div>
  );
};
 
// Step 2. Changed to have router coordinate what is displayed
ReactDOM.render((
  <Router>
    <Route path="/" component={Home} />
  </Router>),
  document.getElementById('root')
);
```

Step 1: In Step 1 above, there are two components that we are importing from **React Router**. We use them in turn.

Step 2: The `Router` (our alias for BrowserRouter) component is the base for our application's routing. It is where we declare how **React Router** will be used. Notice that nested inside the `Router` component we use the `Route` component. The `Route` component has two props in our example: `path` and `component`. The `Route` component is in charge of saying: "when the URL matches this specified `path`, render this specified `component`".

Let's try it. Copy the above code into `src/index.js` and run `npm start` to boot up the application. Once it is running, point your URL to `http://localhost:3000/`. What you'll notice is that when you type in the URL it will render a `<div><h1>Home!</h1></div>`.

### Adding Additional Routes

A <Router> may have only one child element

If you run into this **Problem** just need to wrap routes into an **HTML** Tag

```jsx
ReactDOM.render((
  <Router>
    <div>
      <Route path="/" component={Home} />
      <Route exact path="/about" component={About} />
      <Route exact path="/login" component={Login} />
    </div>
  </Router>),
  document.getElementById('root')
);
```

## refactored 

```jsx
// src/Home.js
import React from 'react';
 
class Home extends React.Component {
  render() {
    return <h1>Home!</h1>
  }
}
 
export default Home;
```

```jsx
// src/About.js
import React from 'react';
 
class About extends React.Component {
  render() {
    return <h1>This is my about component!</h1>;
  }
}
 
export default About;
```

```jsx
// src/Login.js
import React from 'react';
 
class Login extends React.Component {
  render() {
    return (
      <form>
        <h1>Login</h1>
        <div>
          <input type="text" name="username" placeholder="Username" />
          <label htmlFor="username">Username</label>
        </div>
        <div>
          <input type="password" name="password" placeholder="Password" />
          <label htmlFor="password">Password</label>
        </div>
        <input type="submit" value="Login" />
      </form>
    );
  }
}
 
export default Login;
```

```jsx
// src/Navbar.js
import React from 'react'
import { NavLink } from 'react-router-dom';
 
const link = {
  width: '100px',
  padding: '12px',
  margin: '0 6px 6px',
  background: 'blue',
  textDecoration: 'none',
  color: 'white',
}
 
class Navbar extends React.Component {
  render() {
    return (
      <div>
        <NavLink
          to="/"
          /* set exact so it knows to only set activeStyle when route is deeply equal to link */
          exact
          /* add styling to Navlink */
          style={link}
          /* add prop for activeStyle */
          activeStyle={{
            background: 'darkblue'
          }}
        >Home</NavLink>
        <NavLink
          to="/about"
          exact
          style={link}
          activeStyle={{
            background: 'darkblue'
          }}>About</NavLink>
            
        <NavLink to="/login" exact style={link} activeStyle={{ background: 'darkblue' }}
        >Login</NavLink>
      </div>
    )
  }
}
 
export default Navbar;
```

```jsx
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import Home from './Home'
import About from './About'
import Login from './Login'
import Navbar from './Navbar'
import { BrowserRouter as Router, Route } from 'react-router-dom';
 
ReactDOM.render((
  <Router>
    <div>
      <Navbar />
      <Route exact path="/" component={Home} />
      <Route exact path="/about" component={About} />
      <Route exact path="/login" component={Login} />
    </div>
  </Router>),
  document.getElementById('root')
);
```

