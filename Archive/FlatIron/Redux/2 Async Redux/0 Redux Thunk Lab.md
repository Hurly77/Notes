### Part 1: Set Up the Store and Reducer and Action Creator

#### Configuring the Store

To implement Thunk, we'll need to also import `applyMiddleware` from `redux` and `thunk` from `redux-thunk` (package already included in `package.json`). We pass `thunk` into `applyMiddleware()`, and pass *that* in as the second argument for `createStore`:

```jsx
// ./src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
 
import { Provider } from 'react-redux';
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import catsReducer from './reducers/catsReducer.js';
 
const store = createStore(catsReducer, applyMiddleware(thunk))
 
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

#### etting up the Reducer

For our `catsReducer()` function in `./src/reducers/catsReducer.js`, we'll want to set up a switch that handles two action types, `'LOADING_CATS'` and `'ADD_CATS'`.

```jsx
// ./src/reducers/catsReducer.js
 
const catsReducer = (state = { cats: [], loading: false }, action) => {
  switch(action.type) {
    case 'LOADING_CATS':
      return {
        ...state,
        cats: [...state.cats],
        loading: true
      }
    case 'ADD_CATS':
      return {
        ...state,
        cats: action.cats,
        loading: false
      }
    default:
      return state;
  }
}
 
export default catsReducer;
```

We also set up the initial state here. We can see that in the `'LOADING_CATS'` case, `state.loading` becomes `true`, while the rest of `state` is just copied to a new object. In the `'ADD_CATS'` case, `state.loading` becomes `false`, and `state.cats` is set to the `action.cats` payload (HINT: so we know we're expecting a payload object with a `cats` key).

#### Setting up the Action Creator

ow, define your action creator function, `fetchCats()` in `src/actions/catActions`. Remember, Thunk alters the behavior of action creator functions, allowing us to *return* a function that takes in `dispatch`. Inside this function, we can execute asynchronous code, and, once resolved, we can use `dispatch` to update our store with the remote data.

The `fetchCats()` action creator should use `fetch()` to make the web request for your cat pic JSON. It should use a `.then()` function to parse the JSON of the response to this request, and another `.then()` function chained on that to grab the actual collection of cat pic image objects. Something like:

```jsx
fetch('https://learn-co-curriculum.github.io/cat-api/cats.json').then(response => {
  return response.json()
}).then(responseJSON => {
  // instead of logging here, call dispatch and send the cat JSON data to your store
  console.log(responseJSON.images)
})
```

Remember, we built the `catsReducer` to look for two action types. The first, `'LOADING_CATS'`, should be dispatched *before* the `fetch()` request is called. The other type, `'ADD_CATS'`, should be dispatched along with a payload of the cats JSON collection. Judging by the case for `'ADD_CATS'`:

```jsx
...
case 'ADD_CATS':
      return {
        ...state,
        cats: action.cats,
        loading: false
      }
...
```

We can see that the reducer is expecting an action that looks like this:

```jsx
{
  type: 'ADD_CATS',
  cats: // cat data from the cat API
}
```

Putting what we know together, we can start by writing the basic function definition:

```jsx
export const fetchCats = () => {
  return (dispatch) => {
    dispatch({ type: 'LOADING_CATS' })
  }
}
```

The first thing we want to do in this function is send a `dispatch` to indicate we're loading (fetching) the cats:

```jsx
export const fetchCats = () => {
  return (dispatch) => {
    dispatch({ type: 'LOADING_CATS' })
  }
}
```

Then, we call `fetch()`, dispatching the returned data:

```jsx
export const fetchCats = () => {
  return (dispatch) => {
    dispatch({ type: 'LOADING_CATS'})
    fetch('https://learn-co-curriculum.github.io/cat-api/cats.json').then(response => {
      return response.json()
    }).then(responseJSON => {
      dispatch({ type: 'ADD_CATS', cats: responseJSON.images })
    })
  }
}
```

### Part 2: Build the Container Component

Now that Redux and Thunk are set up, it is time to display the retrieved data in our app. First, let's set up the `App` component to read from our Redux store. We'll do this by first importing `connect` from `react-redux`, wrapping the function around `App` on the export line. Then, we'll write a `mapStateToProps()` helper function. This function will be passed into `connect`. `connect` calls this function, passing in the state from the Redux store. Any key/value pairs returned by `mapStateToProps()` will become props in the `App` component. We can use this set up to confirm Redux is correctly creating its initial state and that we're able to access that state in our React components.

```jsx
// src/App.js
import React, { Component } from 'react';
import { connect } from 'react-redux';
 
class App extends Component {
 
  render() {
    console.log(this.props.catPics)
    return (
      <div className="App">
        <h1>CatBook</h1>
        {/* missing component */}
      </div>
    );
  }
}
 
const mapStateToProps = state => {
  return {
    catPics: state.cats,
    loading: state.loading
  }
}
 
export default connect(mapStateToProps)(App)
```

Using the above code, you should see an empty array logged in the console when the app is launched. This is the empty `cats` array in our initial state, now mapped to `this.props.catPics` in `App`.

#### Dispatching the `fetchCats` Action

This is something new, so read carefully...

You might be wondering when/where we will actually dispatch our `fetchCats` action to get all the cat pics into state. We want our cat pics to be fetched when the `App` component is first loaded up. So, we'll enact a common pattern in which we hook into a component lifecycle method to fetch the cat pics.

#### The `componentDidMount` function

The `componentDidMount()` function will *always be called automatically when the component is mounting for the first time*. This is the perfect place to go and get the cat pics.

We need to define our `componentDidMount()` function so that it calls our `fetchCats()` action creator. We also need to write out a `mapDispatchToProps()` function to make `fetchCats()` available. We can then access the function as `this.props.fetchCats()` inside the component and call this when the component mounts:

```jsx
// src/App.js
import React, { Component } from 'react';
import { connect } from 'react-redux';
import { fetchCats } from './actions/catActions'
 
class App extends Component {
 
  componentDidMount() {
    console.log(this.props)
    this.props.fetchCats()
  }
 
  render() {
    console.log(this.props.catPics) // log will fire every time App renders
    return (
      <div className="App">
        <h1>CatBook</h1>
        {/* missing component */}
      </div>
    );
  }
}
 
const mapStateToProps = state => {
  return {
    catPics: state.cats,
    loading: state.loading
  }
}
 
const mapDispatchToProps = dispatch => {
  return {
    fetchCats: () => dispatch(fetchCats())
  }
}
export default connect(mapStateToProps, mapDispatchToProps)(App)
```

Ah! If we check the console, we'll see that `this.props.catPics` is set to `[]` on the first two renders, but on the third, we see an array of 20 cat objects! Notice that we still can call `dispatch` here, even though we're also calling `dispatch` in our action creator.

> **Aside**: Why is `this.props.catPics` set to `[]` on the first two renders? The first render is the initial render, so an empty `catPics` array is always expected. The *second* render, however, occurs when we send our *first* dispatch, `dispatch({type: 'LOADING_CATS'})`. If we logged `this.props.loading` instead of `catPics`, we would see the following:

```js
false
true
false
```

Once you successfully fetch cats, put them in state, grab them from state and pass them to `App` under the `catPics` prop, you're ready to build the `CatList` component.

#### The Presentational Component

We will leave the final task to you - building the `CatList` component. Your container component, `App`, should render the`CatList` component. `App` will pass `catPics` down to `CatList` as a prop. `CatList` should iterate over the cat pics and display each cat pic in an image URL. Remember to use `debugger` to take a look at the `catPics` collection and determine which property of each `catPic` object you will use to populate your `<img>` tag and render the image. In order to get the tests to pass, you will need to wrap your `<img>` tags in a `<div>` tag or something similar.

```jsx
// src/App.js
import React, { Component } from 'react';
import { connect } from 'react-redux';
import { fetchCats } from './actions/catActions'
import CatList from './CatList.js'
 
class App extends Component {
 
  componentDidMount() {
    this.props.fetchCats()
    console.log(this.props)
  }
 
  render() {
    console.log(this.props.catPics) // log will fire every time App renders
    return (
      <div className="App">
        <h1>CatBook</h1>

        {this.props.catPics.length > 1 ? <CatList catPics={this.props.catPics} loading={this.props.loading}/> : null
}      </div>
    );
  }
}
 
const mapStateToProps = state => {
  return {
    catPics: state.cats,
    loading: state.loading
  }
}
 
const mapDispatchToProps = dispatch => {
  return {
    fetchCats: () => dispatch(fetchCats())
  }
}
export default connect(mapStateToProps, mapDispatchToProps)(App)
```



```jsx
import React, {Component} from 'react'

export default class CatList extends Component {


  render() {
    console.log(this.props)
    return (
      <div>
        {this.props.catPics.map((pic)=>{
        return(  
          <li key={pic.id}>
            <img src={pic.url} alt={pic.source_url} width={160} length={160} />
           </li>
        )
        })}
      </div>
    )
  }
}
```



## Conclusion

With all tests passing, you should have a working example of a React + Redux + Thunk application. Of the two components, one is purely presentational, just like a regular React app. The other connects to Redux, but beyond that, it's not any different than a regular React + Redux app. Thunk lets us augment our action creators and handle our asynchronous requests without requiring any major changes to other parts of the application.

## Bonus

While we have a working application, there is one more thing we did not fully implement: handling loading. If you've followed the instructions, you should have access to `this.props.loading` in your `App` component. If we log this value, we should see that it starts off `false`, then becomes `true` briefly before switching back to `false` again.

While content is being fetched, it would be nice to show the user something - often, spinning icons are used, but even just a simple 'Loading...' is enough to show to the user that content is on the way.

How might we use the value of `this.props.loading` to implement a loading message until the cat images arrive?