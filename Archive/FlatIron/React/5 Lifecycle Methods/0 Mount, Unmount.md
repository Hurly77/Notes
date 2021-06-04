##  Pre-Mounting

#### `constructor`

Technically the **`constructor`** is the first function called upon instantiating **any** class in JS, not just React Components. set the initial state of a component. Within the constructor, one can initialize state like so:

```jsx
class App extends React.Component {
 
  constructor() {
    super()
    this.state = {
      key: "value"
    }
  }
 
}
```

In ES7, it is possible to initialize state by simply doing the following inside of your component.

```jsx
class App extends React.Component {
  state = {
    key: "value"
  }
}
```

NOTE: Bear in mind that we call `super` so that we can execute the `constructor` function that is inherited from `React.Component` while adding our own functionality.

It is possible to use the `constructor` to set an initial state that is dependent upon props like so:

```jsx
constructor(props) {
  super(props);
  this.state = {
    color: props.initialColor
  };
}
//source: https://reactjs.org/docs/react-component.html#constructor
```

Note that in contrast to the previous example, we take `props` as an argument to the constructor. This is because we are making use of the props to set an initial state - if we aren't using props to do this, then we need not include `props` as an argument to the constructor.

## Mounting

In the mounting (or DOM creation, or "setup") phase, we have access to two **lifecycle methods**: **`static getDerivedStateFromProps`**, and **`componentDidMount`**

#### `static getDerivedStateFromProps`

The **`static getDerivedStateFromProps`** is called every time a component is rendered, including the first time, during mounting. From the [React blog](https://reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#when-to-use-derived-state):

> `getDerivedStateFromProps` exists for only one purpose. It enables a component to update its internal state as the result of changes in props... We did not provide many examples, because as a general rule, derived state should be used sparingly."

### `componentDidMount`

 called once, but immediately *after* the first `render()` method has taken place. That means that the HTML for the React component has been rendered into the DOM and can be accessed if necessary. This method is used to perform any DOM manipulation or data-fetching that the component might need.

In React, this is where you would set up any long-running processes you want to use in your component, for example fetching data. Suppose we were building a weather app that fetches data on the current weather and displays it to the user. We would want this data to update every 15 seconds without the user having to refresh the page. `componentDidMount` to the rescue!

## Unmounting

In the unmounting (or deletion, or "cleanup") phase, we have just one lifecycle method to help us out: `componentWillUnmount`. The `componentWillUnmount` method is the last function to be called immediately before the component is removed from the DOM. It is generally used to perform clean-up for any DOM-elements or timers created in **`componentDidMount`**.

At a picnic, `componentWillUnmount` corresponds to just before you pick up your picnic blanket. You would need to clean up all the food and drinks you've set on the blanket first or they'd spill everywhere! You'd also have to shut down your radio. After that's all done you would be free to pick up your picnic blanket and put it back in the bag safely. There is no need to make a sandwich right now, we're already leaving.

For a React component, this is where you would clean up any of those long running processes that you set up in `componentDidMount`. In the above data fetching example, all we would have to do is clear the interval so that the weather API would no longer get called every 15 seconds:

```jsx
componentWillUnmount() {  clearInterval(this.interval);}
```

