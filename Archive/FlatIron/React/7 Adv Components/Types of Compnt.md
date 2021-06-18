#### Class Components

all components can be written as class components

class component:

```jsx
import React, { Component } from 'react'
 
class App extends Component {
  render() {
    return <div></div>
  }
}
 
export default App
```

#### Pure Components

a **Pure Component** is just like a class component, the only difference being that pure components don’t have access to `shouldComponentUpdate` method.

```jsx
import React, { PureComponent } from 'react'
 
class App extends PureComponent {
  render() {
    return <div></div>
  }
}
 
export default App
```

**Pure component** is a component given the values(props and state) will always behave the same. If you don't need to fine tune how a class component updates, considered converting most or all of your regular components into pure components.

#### Functional Components

When  a class component is rendered it goes through a series of checks related to its lifecycle. if we **don’t need state or lifecycle** we can avoid checks with a *functional* component

A functional component requires much less than a class component:

```jsx
import React from 'react'
 
const App = props => {
  return (
    <div>{props.greeting}</div>
  )
}
 
export default App
```

**Functional** component *returns* JSX, instead of using `render` method. it does not extent `Component`, so it hasn’t inherited what is needed to store state. a `functional` component can still receive props, but they have to explicitly be written as the argument for the function.

 Often, when we want a component to just *display* content and not worry about any heavy logic, functional components are a great option.

With ES6, we can even shorten functional components to single lines:

```jsx
import React from 'react'
 
const App = props => <div>{props.greeting}</div>
 
export default App
```

Combined with `object destructing` - (to unpack values from arrays, or props from obj, into variables) we can extract out the `greetin` value from `props` to do this:

```jsx
const App = ({ greeting }) => <div>{ greeting }</div>
```

**For instance**  you can write a reusable Button component that has a consistent style but receives props that define its text and click event function:

```jsx
import React from 'react'
 
const Button = ({ handleClick, text })=> <button style="myButton" onClick={ handleClick }>{ text }</button>
 
export default Button
```

Functional components update based on prop changes *or* if their parent component re-renders.

## Container vs Presentation Components

***container*** and ***presentation*** `Components` are not different *types* of **components**, they are a way of thinking, more of a ***how-to*** on organize a React app.

**Presentational components** - are only concerned with displaying Content. they don’t deal with state or have much logic. **EX.**The Button component **from** the **functional component** *section above is a great example of this.*



**Container components** - deal with managing state and class methods.

Imagine for a moment we were designing a navigation bar, full with links and drop down menus, a search form and a brand logo. In React, we can compartmentalize - each piece can be a component (NavLinks, Menu, Search, etc..) and since they all go together, we can create a parent component, that acts as a *container* for everything.

```jsx
import React, { Component } from 'react'
import Logo from './Logo'
import NavLinks from './NavLinks'
import DropMenu from './DropMenu'
import Search from './Search'
 
class NavigationContainer extends Component {
 
  state = {
    query: "",
    username: ""
  }
 
  render() {
    // <>...</> is a a React fragment - it does not render anything to the DOM, but can wrap multiple JSX elements
    return (
      <>
        <Logo />
        <NavLinks />
        <DropMenu username={ this.state.username }/>
        <Search query= {this.state.query } handleChange={ this.handleChange } handleSubmit={ this.handleSubmit }/>
      </>
    )
  }
 
  handleSubmit = event => { ... }
  handleChange = event => { ... }
}
```

**Container components**, having to deal with state, are usually class components**. **

**Presentational components** are most often functional components as they don't need to contain custom methods, relying mainly on props.