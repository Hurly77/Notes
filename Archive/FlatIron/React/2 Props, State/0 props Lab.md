# Reference

**Hint**: In JSX, it is possible to include JavaScript code and calls to functions. A function called in JSX must be wrapped in curly braces and must return a single JSX element. That single JSX element, however, can contain other elements. So, for instance, the component below renders valid JSX:

```jsx
import React from 'react';
 
class App extends React.Component {
 
  generateInnerJSX = () => {
    return (
      <ul>
        <li>Hello</li>
        <li>Goodbye</li>
      </ul>
    )
  }
  render() {
    return (
      <div>
        {this.generateInnerJSX()}
      </div>
    )
  }
}
 
export default App;
```

There is an important exception to this: it is possible to return multiple JSX elements in an *array*. So instead of having to wrap the `li` elements above in a `ul` element, we could write:

```jsx
import React from 'react';
 
class App extends React.Component {
 
  generateInnerJSX = () => {
    return [
      <li>Hello</li>,
      <li>Goodbye</li>
    ]
  }
 
  render() {
    return (
      <div>
        {this.generateInnerJSX()}
      </div>
    )
  }
}
 
export default App;
```

More importantly, because we can return arrays, we can use `.map` to map over data and return an array of elements. The code below will render two `li` elements just like the previous example, but this time using data from an array:

```jsx
import React from 'react';
 
const LIST = ["Hello", "Goodbye"]
 
class App extends React.Component {
 
  generateInnerJSX = () => {
    return LIST.map(item => <li>{item}<li>)
  }
 
  render() {
    return (
      <div>
        {this.generateInnerJSX()}
      </div>
    )
  }
}
 
export default App;
```

Let's see another example. Suppose you have a component called `List` instead of an `li`. We can also map through an array of data and return an array of JSX to dynamically create our `List` components. We can even pass the strings `Hello` and `Goodbye` as props:

```jsx
class List extends React.Component{
  render(){
    return <li>{this.props.content}</li>
  }
}
 
const LIST = ["Hello", "Goodbye"]
 
class App extends React.Component {
  generateInnerJSX = () => {
    return LIST.map(item => <List content={item}/>)
  }
 
  render() {
    return (
      <div>
        {this.generateInnerJSX()}
      </div>
    )
  }
}
```

The above code is the same as below:

```jsx
class App extends React.Component {
  render() {
    return (
      <div>
        {[
          <List content={"Hello"} />,
          <List content={"Goodbye"} />
        ]}
      </div>
    )
  }
}
```

# lab

##### `MovieShowcase`

```jsx
import React, { Component } from 'react';
import MovieCard from './card-components/MovieCard.js'
import movieData from './data.js'

export default class MovieShowcase extends Component {

  generateMovieCards = () => {
    // map over your movieData array and return an array of the correct JSX
    return movieData.map((movie, idx) => {
      return <MovieCard
      key={idx}
      title={movie.title}
      IMDBRating={movie.IMDBRating}
      genres={movie.genres}
      poster={movie.poster}
      />
    })
  }

  render() {
    return (
      <div id="movie-showcase">
        {this.generateMovieCards()}
      </div>
    )
  }
}
```



##### `MovieCard` 

```jsx
import defaultPoster from '../assets/poster-imgs/default.png';
import cmi from '../assets/poster-imgs/choux-and-maru-go-to-istanbul.png';
import cmp1 from '../assets/poster-imgs/choux-and-maru-p1.png';
import cb from '../assets/poster-imgs/chromeboi.png';
import efv from '../assets/poster-imgs/escape-from-vim.png';
import goldeneye from '../assets/poster-imgs/goldeneye.jpg';
import hbmc from '../assets/poster-imgs/handsome-boy-modeling-club.png';
import msts from '../assets/poster-imgs/marus-spinoff-trapped-in-the-sheets.png';
import tkr from '../assets/poster-imgs/terrance-king-of-the-rats.png';
import ttm from '../assets/poster-imgs/the-trash-man.png';

import React, {Component} from 'react';
import CardFront from './CardFront.js';
import CardBack from './CardBack.js';

const posterMap = {
	'choux-maru-istanbul' : cmi,
	'choux-maru-part-1'   : cmp1,
	chromeboi           : cb,
	'escape-from-vim'     : efv,
	goldeneye           : goldeneye,
	'handsome-boy'        : hbmc,
	'marus-spinoff'       : msts,
	'terrance-king'       : tkr,
	'the-trash-man'       : ttm,
	default             : defaultPoster,
};

export default class MovieCard extends Component {
	render() {
		return (
			<div className="movie-card">
				{/* which component should receive which props? */}
				<CardFront poster={posterMap[this.props.poster]} />
				<CardBack title={this.props.title} genres={this.props.genres} IMDBRating={this.props.IMDBRating}/>
			</div>
		);
	}
}

MovieCard.defaultProps = {
	title      : 'Unknown',
	IMDBRating : null,
	genres     : ['No Genre(s) Found'],
	poster     : 'default'
};
```



##### `CardFront`

```jsx
import React, { Component } from 'react';

export default class CardFront extends Component {

  render() {
    return (
      <div className="card-front" style={{backgroundImage: `url(${this.props.poster})`}}>
          
      </div>
    )
  }
}

```



##### `CardBack`

```jsx
import React, { Component } from 'react';
import zero from '../assets/stars/0-stars.png'
import one from '../assets/stars/1-stars.png'
import two from '../assets/stars/2-stars.png'
import three from '../assets/stars/3-stars.png'
import four from '../assets/stars/4-stars.png'
import five from '../assets/stars/5-stars.png'

const imgMapper = {0: zero, 1: one, 2: two, 3: three, 4: four, 5: five}

export default class CardBack extends Component {

  generateRatingElement = () => {
    // implement meeeee! See the readme for instructions
    if(this.props.IMDBRating == null){
      return <h4>no rating found</h4>
    } else {
      return <img src={imgMapper[this.props.IMDBRating]} alt={this.props.title} />
    }
  }

  render() {
    return (
      <div className="card-back">
        <h3 className="title">{this.props.title}</h3>
        <span />
        { this.generateRatingElement() }
        <span />
        <h5 className="genres">
        <small>{this.props.genres}</small>
        </h5>
      </div>
    )
  }
}
```

