# Code Along Using callbacks To Pass information Lab

In React **props** pass info *down* the component tree, `PARENTS TO: CHILDREN`. To propagate info to in the opposite direction we use **CALLBACK FUNCTIONS**, passed down as props from parent components to children. because functions defined in the parent **they will still be in that context if called from a child component**.

This allows the callback to be owned by another component that is not invoking it.

create three components, forming a parent with two children

```
└── Matrix
    ├── ColorSelector
    └── Cell (x625)
```

Behaveior:

- The `ColorSelector` component provides the user interface to select a specific color.
- When a particular `Cell` is clicked, its background color becomes whatever the current selected color is
- parent component, `Matrix`, keep track of the current selected color.

must be Implemented:

- `ColorSelector` has a way to set some 'selected color' in `Matrix` when a user selects a color

- `Cell` has a way to know what the current selected color is when it is clicked

  

## Code-Along

### `Matrix`

**Current** Setup

```jsx
import React, { Component } from 'react';
import learnSymbol from './data.js'
import Cell from './Cell.js'
import ColorSelector from './ColorSelector.js'
 
export default class Matrix extends Component {
 
  constructor() {
    super()
  }
 
  genRow = (vals) => (
    vals.map((val, idx) => <Cell key={idx} color={val} />)
  )
 
  genMatrix = () => (
    this.props.values.map((rowVals, idx) => <div key={idx} className="row">{this.genRow(rowVals)}</div>)
  )
 
  render() {
    return (
      <div id="app">
        <ColorSelector />
        <div id="matrix"><!-- renders a div containing the ColorSelector component and another div. Within this nested div is a function call to this.genMatrix()
The actual color value stored in the data is passed into Cell as color={val}.
-->
          {this.genMatrix()}
        </div>
      </div>
    )
  }
}
 
Matrix.defaultProps = {
  values: learnSymbol
}
```

Looking a `Cell` Component`color` prop is used to set the initial state of the component

```JSx
import React, { Component } from 'react';

export default class Cell extends Component {
  
  constructor(props) {
    super(props)
    this.state = {
      color: this.props.color
    }
  }
  
  render() {
    return (
      <div className="cell" style={{backgroundColor: this.state.color}}>
      </div>
    )
  }
  
}
```

data is passed into `Matrix` as an array of arrays of strings. On render, this data is mapped to JSX elements. *(With some CSS help,)* these elements form rows of squares on the screen.



`ColorSelector` component, which renders a row of colored `div`s

```jsx
import React, { Component } from 'react';

export default class ColorSelector extends Component {

  makeColorSwatches = () => (
    ["#F00", "#F80", "#FF0", "#0F0", "#00F", "#508", "#90D", "#FFF", "#000"].map((str, idx) => {
      return <div key={idx} className="color-swatch" style={{backgroundColor: str}}/>
    })
  )

  render() {
    return (
      <div id="colorSelector">
        {this.makeColorSwatches()}
      </div>
    )
  }
}
```

The `ColorSelector` component, as suggested by its name, should contain the interface for selecting a color. Once a color is selected, clicking on any particular `Cell` should cause that `Cell` to change to the selected color.

### Update the Matrix Component

`Matrix` component needs to have the following:

- A way for `Matrix` to keep track of the **selected color** (think *state!*)
- A method that takes in a single argument of a hexadecimal color string (i.e. '#fff') and sets the **selected color** to that

 figure out how to use the component's state, as well as the method that will *update* that state, in the `ColorSelector` and `Cell` components.

#### Set Up State

In `src/Matrix.js`, there is no `state` set up. As we need a place to keep track of the selected color, let's add it here:

```jsx
// src/Matrix.js
...
 
constructor() {
  super()
  this.state = {
    selectedColor: '#FFF'
  }
}
 
...
```

#### Create a Method to Update State

```jsx
// src/Matrix.js
...
 
setSelectedColor = (newColor) => {
  this.setState({
    selectedColor: newColor
  })
}
 
...
```

this method updates `selectedColor` with whatever is passed into it as an argument.

#### Pass Data and Callbacks to Children

`ColorSelector` will need access to `setSelectedColor` We can pass the needed function down as a prop:

```Jsx
// src/Matrix.js
...
 
render() {
  return (
    <div id="app">
      <ColorSelector setSelectedColor={this.setSelectedColor} />
      <div id="matrix">
        {this.genMatrix()}
      </div>
    </div>
  )
}
```



`cell` only needs to know the currently selected color. Not to Change it.

```jsx
// src/Matrix.js
...
genRow = (vals) => (
  vals.map((val, idx) => <Cell key={idx} color={val} selectedColor={this.state.selectedColor} />)
)
...
```

After all the changes, `Matrix` looks like this:

```jsx
import React, { Component } from 'react';
import learnSymbol from './data.js'
import Cell from './Cell.js'
import ColorSelector from './ColorSelector.js'
 
export default class Matrix extends Component {
 
  constructor() {
    super()
    this.state = {
      selectedColor: '#FFF'
    }
  }
 
  setSelectedColor = (newColor) => {
    this.setState({
      selectedColor: newColor
    })
  }
 
  genRow = (vals) => (
    vals.map((val, idx) => <Cell key={idx} color={val} selectedColor={this.state.selectedColor} />)
  )
 
  genMatrix = () => (
    this.props.values.map((rowVals, idx) => <div key={idx} className="row">{this.genRow(rowVals)}<div>)
  )
 
  render() {
    return (
      <div id="app">
        <ColorSelector setSelectedColor={this.setSelectedColor} />
        <div id="matrix">
          {this.genMatrix()}
        </div>
      </div>
    )
  }
 
}
 
Matrix.defaultProps = {
  values: learnSymbol
}
```

#### `ColorSelector`

The `ColorSelector` component already has some basic `div`s rendering:

```jsx
// src/ColorSelector.js
...
makeColorSwatches = () => (
  ["#F00", "#F80", "#FF0", "#0F0", "#00F", "#508", "#90D", "#FFF", "#000"].map((str, idx) => {
    return <div key={idx} className="color-swatch" style={{backgroundColor: str}}/>
  })
)
 
render() {
  return (
    <div id="colorSelector">
      {this.makeColorSwatches()}
    </div>
  )
}
...
```

We need to update this code so that when any one of these `div`s is clicked the hexadecimal color value of that `div` becomes the selected color in `Matrix`. For click events, we know we'll have to add an event and provide a callback on the `div` element itself:

```jsx
return <div onClick={this.callback} key={idx} className="color-swatch" style={{backgroundColor: str}}/>
```

Inside this callback, we'll call `this.props.setSelectedColor()`, but where would this callback function need to be defined?

This time is a little different - we'll need to write the function inside the `map` to access the color values needed:

```jsx
...
makeColorSwatches = () => (
  ["#F00", "#F80", "#FF0", "#0F0", "#00F", "#508", "#90D", "#FFF", "#000"].map((str, idx) => {
    let callback = () => this.props.setSelectedColor(str)
    return <div onClick={this.callback} key={idx} className="color-swatch" style={{backgroundColor: str}}/>
  })
)
...
```

Clicking on a particular `div` inside `ColorSelector` should now set state in `Matrix`.

#### `Cell`

we now need to configure our `Cell` component so that when it is clicked, it changes color to the currently selected color.

For `Cell`, we can set up another click event, just like in `ColorSelector`, only this time, we'll use a `handleClick` class method like we've seen before:

```jsx
import React, { Component } from 'react';
 
export default class Cell extends Component {
 
  constructor(props) {
    super(props)
    this.state = {
      color: this.props.color
    }
  }
 
  handleClick = () => {
    this.setState({
      color: this.props.selectedColor
    })
  }
 
  render() {
    return (
      <div onClick={this.handleClick} className="cell"
           style={{backgroundColor: this.state.color}}
      >
      </div>
    )
  }
 
}
```

s