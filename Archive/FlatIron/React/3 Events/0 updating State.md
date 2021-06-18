# notes

**State is only** reserved for data that changes in our component and is visible in the UI.

With React we don’t modify the state directly, with `this.state` we use `this.setState()` this function is available to all Components in React. This way we can let react know that the component state has changed and the UI is likely to change as well.

**For example**: 

```jsx
// src/components/ClickityClick.js
import React from 'react';
 
class ClickityClick extends React.Component {
  constructor() {
    super();
 
    // Define the initial state:
    this.state = {
      hasBeenClicked: false
    };
  }
 
  handleClick = () => {
    // Update our state here...
  };
 
  render() {
    return (
      <div>
        <p>I have {this.state.hasBeenClicked ? null : 'not'} been clicked!</p>
        <button onClick={this.handleClick}>Click me!</button>
      </div>
    );
  }
}
 
export default ClickityClick;
 
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
 
import ClickityClick from './components/ClickityClick';
 
ReactDOM.render(<ClickityClick />, document.getElementById('root'));
```

Pass in an object to `this.setState()` The object will be merged with the current State.

```jsx
// src/components/ClickityClick.js
...
 
handleClick = () => {
  this.setState({
    hasBeenClicked: true
  })
}
 
...
```

## How State Gets Merged

we don’t have to pass in the whole state, only the property we want updated. **EX:**

```JSX
{
  hasBeenClicked: false,
  currentTheme: 'blue',
}
//if we merge the state with existing state like above, we'd get a new state like so:
  {
  hasBeenClicked: true,
  currentTheme: 'blue',
}
```

**IMPORTANT:** we can only merge things on the first level.

if we try to update only on field with several empty fields this would happen.

```jsx
//Object, we want to update
{
  theme: 'blue',
  addressInfo: {
    street: null,
    number: null,
    city: null,
    country: null
  },
----------------------------------
//update state
}
this.setState({
  addressInfo: {
    city: 'New York City'
  }
});
-----------------------------------
//result from update
{
  theme: 'blue',
  addressInfo: {
    city: 'New York City',
  },
}
```

Because `setState()` doesn’t deeply merge the object we overwrite any key/values, that are null.

##### OPTIONS FOR UPDATING NULL OBJECTS KEYS WITH OUT VALUES

we can either use `Object.assign()`

```jsx
this.setState({
  addressInfo: Object.assign({}, this.state.addressInfo, {
    city: 'New York City'
  })
});
```

**Or ** we can use the spread operator **RECOMMENDED**

```jsx
this.setState({
  addressInfo: {
    ...this.state.addressInfo,
    city: 'New York City'
  }
});
```

> The [spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) syntax can be used in JavaScript to 'de-compose' objects and arrays. When used on an object as we see above, `...this.state.addressInfo` returns all the keys and values from within that object. We're saying `addressInfo` should be equal to all the keys and values that make up `addressInfo`, and, in addition, there should be `city` key with the value `New York City`. If there is already a `city` key inside `addressInfo`, it will be overwritten. If it doesn't exist, it will be added.

Both of these would result in the state updating to this shape:

```JSX
{
  theme: 'blue',
  addressInfo: {
    street: null,
    number: null,
    city: 'New York City',
    country: null
  },
}
```

## Setting state is not synchronous

Because setting State is asynchronous, we get unexpected behavior

```jsx
handleClick = () => {
  this.setState({
    hasBeenClicked: true
  })
  console.log(this.state.hasBeenClicked); // prints false
}
```

console will output `false` because **State changes** happen asynchronously.

If we want to access new state after is has been updated, we can optionally add a callback

```jsx
handleClick = () => {
  this.setState({
    hasBeenClicked: true
  }, () => console.log(this.state.hasBeenClicked)) // prints true
}
```

## State Changes vs. Prop Changes

**Props/State** - Changes in state and/or props will both trigger a re-render of our React component.

**Props**  - Changes in props can only be changed externally **by** the **parent** or grandparent component. 

**State** - State can only be changed internally, due to components changing their own state

## Updating State Based on the Previous State

When making changes to State we **should not use `this.state` inside of `setState`** is not synchronous — multiple `setState` calls may be grouped together into one update.

##### OPTIONS FOR CHANGING PREVIOUS STATE

One way is to handle the logic this.state` outside of `setState

```JSX
import React, {Component} from 'react';
 
class ButtonCounter extends Component {
  constructor() {
    super()
    // initial state has count set at 0
    this.state = {
      count: 0
    }
  }
 
  handleClick = () => {
    // when handleClick is called, newCount is set to whatever this.state.count is plus 1 PRIOR to calling this.setState
    let newCount = this.state.count + 1
    this.setState({
      count: newCount
    })
  }
 
  render() {
    return (
      <div>
        <h1>{this.state.count}</h1>
        <button onClick={this.handleClick}>Click Me</button>
      </div>
    )
  }
}
 
export default ButtonCounter
```

React provides a built in solution instead of the above. Instead of passing an object into `setState`, we can also pass a function. hat function, when called inside `setState` will be passed the component state from when that `setState` was called. 

this is called **previous State**

```jsx
handleClick = () => {
    this.setState(previousState => {
      return {
        count: previousState.count + 1
      }
    })
  }
```

##### Using *previous state* with Boolean

toggle between `true` and `false` repeatedly?

```jsx
import React from 'react';
 
class LightSwitch extends React.Component {
  constructor() {
    super();
 
    // Initial state is defined
    this.state = {
      toggled: false
    };
  }
 
  // when handleClick is called, setState will update the state so that toggle is reversed
  handleClick = () => {
    this.setState(previousState => {
      return {
        toggled: !previousState.toggled
      }
    })
  }
 
  render() {
    return (
      <div>
        <button onClick={this.handleClick}>{this.state.toggled ? "ON" : "OFF"}</button>
      </div>
    );
  }
}
 
export default LightSwitch;
```

# Lab

1. In the `components/DigitalClicker.js` file, create a `DigitalClicker` React component.
2. This component has an initial state property called `timesClicked`, which is initially defined as 0.
3. The component renders out a button with a label that shows the `timesClicked` value. This means that, at the start, your button should just say `0`.
4. Whenever the button is clicked, update the state by incrementing the `timesClicked` by 1.

```jsx
import React, {Component} from 'react'

export default class DigitalClicker extends Component {
  constructor(){
    super()
    this.state = {
      timesClicked: 0
    }
  }

  handleClick = () => {
    this.setState(previousState => {
      return {
        timesClicked: previousState.timesClicked + 1
      }
    }) 
  }

  render(){
    return(
    <button onClick={this.handleClick}>{this.state.timesClicked}</button>
    )
  }
}
```

1. In the `components/YouTubeDebugger.js` file, create a `YouTubeDebugger` React component.
2. This component has several state properties. The initial state shape looks like this:
3. Create a button with the class `'bitrate'`. Clicking this button changes the `settings.bitrate` state property to `12`.
4. Create a button with the class `'resolution'`. Clicking this button changes the `settings.video.resolution` state property to `'720p'`.

```jsx
import React, {Component} from 'react';

export default class YouTubeDebugger extends Component {
	constructor() {
    super()
		this.state = {
      errors: [],
      user: null,
      settings: {
        bitrate: 8,
        video: {
          resolution: '1080p'
        }
      }
    }
  }
  
  bitrateClick = () => {
    this.setState({
      settings:{
        ...this.state.settings,
        bitrate: 12
      }
    })
  }

  resolutionClick = () => {
    this.setState({
      settings: {
        ...this.state.settings,
        video:{
          resolution: '720p'
        }
      }
    })
  }

	render() {
		return (
			<div>
				<button onClick={this.bitrateClick} className="bitrate">{this.state.settings.bitrate}</button>
				<button onClick={this.resolutionClick} className="resolution">{this.state.settings.video.resolution}</button>
			</div>
		);
	}
}
```