## React's Event System

React makes use of basic HTML events by wrapping them in something called `SyntheticEvent`s

## How to add event handlers

Consider the following component:

```jsx
class Tickler extends React.Component {
 
  tickle = () => {
    console.log('Tee hee!')
  }
 
  render() {
    return (
      <button>Tickle me!</button>
    )
  }
}
//to trigger the event
<button onClick={this.tickle}>Tickle me!</button>
```

we pass the *function object itself* and **do not invoke the function**

**You will commonly see React component methods defined with arrow functions.** This is because we often want to access the `this` keyword within the methods themselves.

# lab

## Deliverables

- invokes the `drawChromeBoiAtCoords` method within `handleMouseMove`, passing the captured x and y values of the mouse from the event       
- has an event listener for clicks on the <canvas> element that triggers `toggleCycling`
- has an event listener for key presses on the <canvas> element that triggers `resize`
- when the 'a' key is pressed, `resize` is invoked with the argument of '+'
- when the 's' key is pressed, `resize` is invoked with the argument of '-'

Code

```jsx
import React, { Component } from 'react';
import { drawChromeBoiAtCoords, toggleCycling, resize } from './canvasHelpers.js'


export default class ChromeBoisDomain extends Component {
  
  handleMouseMove = (event) => {
    /* TODO: This method should capture the `x` and `y` coordinates of the mouse
     * from the event and use them to invoke the `drawChromeBoiAtCoords`
     * function that has been provided and is already imported
     * (`drawChromeBoiAtCoords` expects two arguments, an x and a y coordinate)
     */
   let x = event.clientX
   let y = event.clientY
   drawChromeBoiAtCoords(x, y)
  }
  handleClick = () => {
    toggleCycling()
  }
  /* TODO: Create an event handler which, when fired, invokes the provided
  * `toggleCycling` function with no arguments. Don't forget the click event
  * listener that should fire it!
  */
 
 handleKey = (event) => {
   const type = (event.key == 'a' ? '+' :  '-')a
  resize(type)
 }
 /* TODO: Add an event listener to the `<canvas>` element to capture when a key
 /* is pressed. When a key is pressed, an event handler should invoke the
 /* provided `resize` function with a single argument of either '+' or '-'
 /* if the key pressed was 'a', then it should call `resize` with '+'
 /* if the key pressed was 's', then it should call `resize` with '-' 
 */
  
  render() {
    return (
      <canvas 
      onMouseMove={this.handleMouseMove}
      onClick={this.handleClick}
      onKeyPress={this.handleKey}
      width='900'
      height='600'
      tabIndex="0">
      </canvas>
    )
  }
}
```

## canvas helpers

```js
// nothing needs to change here. These helpers abstracted here just to keep
// lesson focused on event handling and not hacky HTML5 canvas nonsense

let colors = []
let def = null
let cycling = false
let idx = 0
let [sizeX, sizeY] = [95, 121]

export function init() {
  
  def = document.createElement("img")
  const green = document.createElement("img")
  const red = document.createElement("img")
  const yellow = document.createElement("img")

  def.src = "chrome-boi.png"
  green.src = "chrome-boi-green.png"
  red.src = "chrome-boi-red.png"
  yellow.src = "chrome-boi-yellow.png"
  
  colors = [green, red, yellow]
}

export function drawChromeBoiAtCoords(x, y) {
  
  const canvas = document.querySelector("canvas") // sloppy but we haven't introduced lifecycle methods and canvas wouldn't be rendered
  const ctx = canvas.getContext("2d")
  const rect = canvas.getBoundingClientRect()
  const [cX, cY] = [rect.left, rect.top]
  
  let img
  if (cycling) {
    img = colors[idx]
    idx = (idx + 1) % 3
  } else {
    img = def
  }
  
  ctx.drawImage(img, x - cX - 50, y - cY - 80, sizeX, sizeY)
}

export function toggleCycling() {
  cycling = !cycling
}

export function resize(type) {
  const multiplier = (type === "+") ? 1.1 : 0.9
  sizeX *= multiplier
  sizeY *= multiplier
}
```

index.js

```Jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import { init } from './canvasHelpers'
import ChromeBoisDomain from './ChromeBoisDomain.js'

init()

ReactDOM.render(<ChromeBoisDomain />, document.getElementById('root'));
```

