## **State**

**State** -  *is data that is mutated in your component.* A component’s state, unlike a component’s props, **can change** during the component’s life.

**State** provides us with a way to **maintain** and **update** **information** within a component **without** requiring its **parent** to somehow send updated information.

the **component** could **increment** **itself** without needing any fussy prop passing:

```jsx
class MyComp extends React.Component {
 
  // we use the constructor to set the INITIAL STATE
  constructor() {
    super()
    this.state = {
      count: 0
    }
  }
 
  // our increment method makes use of the 'setState' method, which is what we use to alter state
  increment = () => {
    const newCount = this.state.count + 1
    this.setState({
      count: newCount
    })
  }
 
  render() {
    return (
      <div onClick={this.increment}>
        {this.state.count}
      </div>
    )
  }
}
```

We set up an initial value of state in the `constructor()`, `this.state = {count: 0}` is saying this instance of `MyComp` should have a property called state that has a value of `{count: 0}`. Also, we should call `super()` in constructor since we are inheriting from another calls via the `extends` keyword

**Keep in mind that**: => React **events** are written as attributes inside a `JSX tag` and are name using `camelCase`

We write is as an arrow function to bind the value of `this` to be our instance of `MyComp`. then, when we say `this.setState()`, we are really just saying to set the state of `MyComp`

## Initial State and 'setState()'

#### Initial State

The `constructor` method runs **first** when a component is made (and `render()`runs later!)

Commit This to heart

> WE set **initial state** in the **constructor** because it runs **first**

After providing component with **initial state** we can manipulate it later using the method: `setState()`

#### `setState()`

`SetState()` - **sets/updates** STATE! **very important** - `setState()` sets state *asynchronously*  

(pay close attention to the `console.log()`s:

```jsx
class App extends Component {
 
  constructor() {
    super()
    this.state = {
      count: 0
    }
  }
 
  increment = () => {
    console.log(`before setState: ${this.state.count}`)
 
    this.setState({
      count: this.state.count + 1
    })
 
    console.log(`after setState: ${this.state.count}`)
  }
 
  render() {
    return (
      <div onClick={this.increment}>
        {this.state.count}
      </div>
    )
  }
}

//CONSOLE LOG
-------------------------------------------
	=========================================
	before setState: 0
    =========================================
    after setState: 0
    =========================================
//The component finishes executing the `increment` function in full before updating
```

what we are seing is `setState()` functioning *asynchronously*. The component finishes doing its current task *before* updating the state.

**CAUTION :** Component state should be use sparingly, State adds (sometimes unnecessary) complexity and can be very easy to lose track of.

**Remember:** state is for values that are expected to change during the components life

# lab

```jsx
import React, {Component} from 'react';
import Cell from './Cell';

export default class Matrix extends Component {
	genRow = (vals) => {
		return vals.map((val) => <Cell value={val} />); // replace me and render a cell component instead!
	};

	genMatrix = () => {
		return this.props.values.map((rowVals) => <div className="row">{this.genRow(rowVals)}</div>);
	};

	render() {
		return <div id="matrix">{this.genMatrix()}</div>;
	}
}
Matrix.defaultProps = {
	values : [
		['#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00'],
		['#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00'],
		['#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00'],
		['#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00'],
		['#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00'],
		['#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00'],
		['#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00'],
		['#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00'],
		['#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00'],
		['#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00', '#F00'],
	],
};
```

```jsx
import React, {Component} from 'react'

export default class Cell extends Component {
    constructor(props){
      super()
      this.state = {
        color: props.value
      }
    }

    colorChange = () => {
      this.setState({
        color: this.state.color = '#333'
      })
    }

    render(){
      return (
        <div className="cell" onClick={this.colorChange} style={{backgroundColor: this.state.color}}>
          {this.state.color}
        </div>
      )
    }
}
```

