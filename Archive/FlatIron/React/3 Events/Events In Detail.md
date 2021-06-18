

## Accessing event data

```jsx
export default class Clicker extends React.Component {
 
  handleClick = (event) => {
    console.log(event.type); // prints 'click'
  }
 
  render() {
    return (
      <button onClick={this.handleClick}>Click me!</button>
    );
  }
}
```

if we wanted to get the target of an event, we'd use `event.target`. If we want to prevent a default action whenever an event happens, we call `event.preventDefault()`

## Event pooling

**Event pooling**  -  means that whenever an event fires, its event data (an object) is sent to the callback. The object is then immediately cleaned up for later use. This is what we mean by 'pooling': the event object is in effect being sent back to the pool for use in a later event.

If we click the button of our `Clicker` component and then inspect the logged out object in our console, we notice that all properties are `null` again. By the time we inspect the object in our browser, the **event object** will have already been **returned** to the **pool**. This means that **we can't** **access** **event data** in an **asynchronous manner** by **saving** it in the **state**, or running a timeout and *then* accessing the event again.

You usually don't need to access your event data in an asynchronous manner like described above, but **if you do**, there are two options: you either store the data you need in a variable (e.g. `const target = event.target`), *or* we can make the event persistent by calling that method: `event.persist()`.

# lab

#### `CoordinatesButton`

1. In the `components/CoordinatesButton.js` file, create a `CoordinatesButton` React component.
2. This component takes in one prop: `onReceiveCoordinates`. This prop is a *function* passed down from `index.js`. This function is currently just logging whatever is passed into it.
3. Within `CoordinatesButton`, render a button. On click of the button, create an array with two elements: the X and Y coordinates of the mouse. Find these using the event data.
4. Pass this event data in as the argument for the `onReceiveCoordinates` prop.
5. If successful, the current x,y position of your mouse should be logged.

#### `DelayedButton`

1. In the `components/DelayedButton.js` file, create a `DelayedButton` React component
2. This component takes two props: `onDelayedClick` (a function), and `delay` (a number).
3. Create a button that, when clicked, will pass the click event to the `onDelayedClick` prop *within* a `setTimeout()`. The `setTimeout()` should be set to `this.props.delay`.
4. If successful, the event will be logged to the console once the timeout has finished.
5. Hint: If you having trouble with this feature, remember event pooling in React. By the time the setTimeout fires, the event object will have already been returned to the pool. So how can we fix that?

```jsx
import React, {Component} from 'react'

export default class DelayedButton extends Component {
  handleClick = (event) => {
    event.persist()
    setTimeout(() => {
      this.props.onDelayedClick(event)
    }, this.props.delay)
  }

  render(){
    return(
      <button onClick={this.handleClick}>
        Delayed Button
      </button>
    )
  }
}
```

```jsx
import React, {Component} from 'react'

export default class CoordinatesButton extends Component {
  
  handleClick = (event) => {
    this.props.onReceiveCoordinates([event.clientX, event.clientY])
  }

  
  render(){
    return(
      <button onClick={this.handleClick}>Coordinates Button</button>
    )
  }
}
```

