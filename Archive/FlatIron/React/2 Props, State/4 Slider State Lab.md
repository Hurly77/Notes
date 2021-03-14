```jsx
import React, {Component} from 'react'

export default class Bomb extends Component {
  constructor(props) {
    super(props)
    this.state = {
      secondsLeft: props.initialCount
    }  
  }

  render(){
    const c = this.state.secondsLeft
    const a = `${this.state.secondsLeft} seconds left before I go boom!`
    const b = 'Boom!'
    return(
      <div>
        {(c == 0) ? b:a}
      </div>
    )
  }
}
```

```jsx
import React, {Component} from 'react';

export default class ImageSlider extends Component {
  constructor(){
    super()
    this.state = {
      currentSlideIndex: 0
    }
  }

  render(){
    return(
      <div>
        {`I am on slide ${this.state.currentSlideIndex}`}
      </div>
    )
  }
}

```

