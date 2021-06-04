#### `componentDidUpdate` and React Refs

This method is called after every component re-render. It won’t be called when the component is mounted. The major use case for `componentDidUpdate` is to allow for ‘post-processing’ actions.

because there are specific use cases, React has provided us a way to get access to DOM elements using something called a `ref`. You can see an example of a ref in Timer:

```jsx
constructor() {
  super()
  this.timer = React.createRef()
  ...
}
 
...
 
render() {
  const { time, color, className, logText } = this.state
  return (
    <section className="Timer" style={{background: color}} ref={this.timer}>
    ...
  )
}
```

In Timer, a class variable, `this.timer`, is initialized in the constructor. An attribute, `ref`, is then *added* to a specific JSX element, in this case, `section`.

Once the Timer component mounts, it is possible to access the DOM node, `section`, using the associated `ref` attribute. To access the DOM using `this.timer`, we just need to write `this.timer` followed by `current`:

```jsex
console.log(this.timer.current);
//outputs <section class="Timer" ... > ... </section>
console.log(this.timer.current.style.background);
//outputs the background color, something like rgb(62, 132, 219)
```

With `this.timer.current.style`, we can access and modify many style properties. We can and already are modifying the style of our Timer components using state (the background color is set by state). Using a ref in `componentDidUpdate` to change style properties *will override* any styling set up in the `render()`, but won't set state.

To pass the first test in this lab, write `componentDidUpdate` within Timer. Use the provided ref to manipulate the DOM node to visually confirm. One suggestion:

```jsx
this.timer.current.style.color =
  "#" + Math.floor(Math.random() * 16777215).toString(16);
```

**A Note About Refs:** React builds a *virtual DOM* when it renders JSX, allowing it to update the actual DOM in a very efficient way. Accessing the DOM directly comes with some caution as it circumvents React's default behavior. For this reason, it is better to handle most style changes using an external CSS file or in-line within JSX, if possible.



#### `shouldComponentUpdate`

`shouldComponentUpdate` takes in two arguments, the *next* props and state from the potential update. That is to say, when a component is *about to* update, it calls `shouldComponentUpdate`, passing in the new props and state. Whatever the return value is will determine if the component will continue with the update process. Because of this, from within `shouldComponentUpdate`, we have access to both the *current* props and state, accessible with `this.state` and `this.props`, and the *next* props and state, represented below as `nextProps` and `nextState`:

```jsx
shouldComponentUpdate(nextProps, nextState) {
  if (this.state.time === nextState.time) {
    return false
  }
  return true
}
```

Include the above `shouldComponentUpdate` method in Timer. In regards to the Timer component updating, the only time we really need to update is when `this.state.time` changes. Including this code prevents unnecessary updates being caused by App's state changes.

The result is that the DOM changes you've made in `componentDidUpdate` will only take effect when a Timer increments.

Run `learn` to confirm you've passed the tests for adding `componentDidUpdate` and `shouldComponentUpdate` to the Timer component.



## Bonus - Pure Components

If you've passed your tests, there is an additional bit of information you may find useful - [Pure Components](https://reactjs.org/docs/react-api.html#reactpurecomponent). Pure components **do not implement `shouldComponentUpdate`**. Instead, a pure component automatically does a comparison of current and next props and state, and only updates if it registers a change.

The only change that registers for our Timer components is the change in `this.state.time`. This means that instead of including this:

```jsx
shouldComponentUpdate(nextProps, nextState) {  if (this.state.time === nextState.time) {    return false  }  return true}
```

We can just change from a Component to a PureComponent:

```jsx
import React, { PureComponent } from 'react'; class Timer extends PureComponent {  ...}
```

And get an instant, easy reduction in unnecessary updates!