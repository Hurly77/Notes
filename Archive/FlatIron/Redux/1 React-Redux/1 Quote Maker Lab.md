## top-level

`App.js`

```jsx
import React, { Component } from 'react';
import QuoteForm from './components/QuoteForm.js'
import Quotes from './containers/Quotes.js'

class App extends Component {
  render() {
    return (
      <div className="container-fluid">
        <div className="row title justify-content-center" style={{ paddingTop: '12px' }}>
          <h1>Quote Maker</h1>
        </div>
        <hr />
        <QuoteForm />
        <Quotes />
      </div>
    );
  }
}

export default App;
```

`index.js`

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import { Provider } from 'react-redux';
import { createStore } from 'redux'
import rootReducer from './reducers/index'

let store = createStore(rootReducer)

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

`store.js`

```jsx
import { createStore } from 'redux'
import rootReducer from './reducers/index'

export function configureStore(){
  return createStore(
    rootReducer, 
    window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
  );
}

export const store = configureStore()
```

## actions

`actions/quotes.js`

```jsx
export const addQuote = (quote) => {
  return {
    type: "ADD_QUOTE",
    quote: quote
  }
}

export const removeQuote = (quoteId) => {
  return {
    type: "REMOVE_QUOTE",
    quoteId: quoteId
  }
}

export const upvoteQuote = (quoteId) => {
  return {
    type: "UPVOTE_QUOTE",
    quoteId: quoteId
  }
}

export const downvoteQuote = (quoteId) => {
  return {
    type: "DOWNVOTE_QUOTE",
    quoteId: quoteId
  }
}
```

## components

`components/QuoteCard.js`

```jsx
import React from 'react';

const QuoteCard = (props) => 
<div>
    <div className="card card-inverse card-success card-primary mb-3 text-center">
      <div className="card-block">
        <blockquote className="card-blockquote">
          <p>{props.quote.content}</p>
          <footer>{props.quote.author}</footer>
        </blockquote>
      </div>
      <div className="float-right">
        <div className="btn-group btn-group-sm" role="group" aria-label="Basic example">
          <button type="button" className="btn btn-primary" onClick={() => props.upvoteQuote(props.quote.id)}>Upvote</button>
          <button type="button" className="btn btn-secondary" onClick={() => props.downvoteQuote(props.quote.id)} >Downvote</button>
          <button type="button" className="btn btn-danger" onClick={() => props.removeQuote(props.quote.id)} ><span aria-hidden="true">&times;</span>
          </button>
        </div>
        {props.quote.votes}
      </div>
    </div>
  </div>;

export default QuoteCard;
```

`components/QuoteForm.js`

```jsx
import React, {Component} from 'react';
import uuid from 'uuid';
import {connect} from 'react-redux';
import {addQuote} from '../actions/quotes';

class QuoteForm extends Component {
	state = {
		content : '',
		author  : '',
		votes   : 0
	};

	handleOnChange = (event) => {
    const {value, name} = event.target;
    this.setState({
      [name] : value
    })
	};

	handleOnSubmit = (event) => {
    event.preventDefault()
    const newQuote = {...this.state, id: uuid()}
    this.props.addQuote(newQuote)
    this.setState({
      author: '',
      content: ''
    })
	};

	render() {
		return (
			<div className="container">
				<div className="row">
					<div className="col-md-8 col-md-offset-2">
						<div className="panel panel-default">
							<div className="panel-body">
								<form className="form-horizontal" onSubmit={(event) => this.handleOnSubmit(event)}>
									<div className="form-group">
										<label htmlFor="content" className="col-md-4 control-label">
											Quote
										</label>
										<div className="col-md-5">
											<textarea className="form-control" name="content" value={this.state.content} onChange={this.handleOnChange} />
										</div>
									</div>
									<div className="form-group">
										<label htmlFor="author" className="col-md-4 control-label">
											Author
										</label>
										<div className="col-md-5">
											<input className="form-control" name="author" type="text" value={this.state.author} onChange={this.handleOnChange} />
										</div>
									</div>
									<div className="form-group">
										<div className="col-md-6 col-md-offset-4">
											<button type="submit" className="btn btn-default" >
												Add
											</button>
										</div>
									</div>
								</form>
							</div>
						</div>
					</div>
				</div>
			</div>
		);
	}
}

//add arguments to connect as needed
export default connect(null, {addQuote})(QuoteForm);
```

## containers

`containers/Quotes.js`

```jsx
import React, { Component } from 'react';
import { connect } from 'react-redux';
import QuoteCard from '../components/QuoteCard';
import {upvoteQuote, downvoteQuote, removeQuote } from '../actions/quotes'

class Quotes extends Component {
  render() {
    const {quotes, upvoteQuote, downvoteQuote, removeQuote} = this.props
    return (
      <div>
        <hr />
        <div className="row justify-content-center">
          <h2>Quotes</h2>
        </div>
        <hr />
        <div className="container">
          <div className="row">
            <div className="col-md-4">
            <div>
              {quotes.map(quote => {
              return <QuoteCard key={quote.id} quote={quote} upvoteQuote={upvoteQuote} downvoteQuote={downvoteQuote} removeQuote={removeQuote} />
              })}
            </div>
            </div>
          </div>
        </div>
      </div>
    );
  }
}

const mapToStateProps = (state) => {
  return {
    quotes: state.quotes
  }
}
//add arguments to connect as needed
export default connect(mapToStateProps, {upvoteQuote, downvoteQuote, removeQuote})(Quotes)
```

## reducers

`reducers/index.js`

```jsx
import { combineReducers } from 'redux';
import quotes from './quotes';

export default combineReducers({
  quotes
});

```

`reducers/quotes.js`

```jsx
function quoteReducer(state = [], action){
  let idx;
  let quote;
  switch(action.type){
    case 'ADD_QUOTE':
      return state.concat(action.quote)
     case 'REMOVE_QUOTE':
       return state.filter(quote => quote.id !== action.quoteId) 
     case 'UPVOTE_QUOTE':
       idx = state.findIndex(quote => quote.id === action.quoteId)
       quote = state[idx]
 
       return [
         ...state.slice(0, idx),
         Object.assign({}, quote, {votes: quote.votes += 1}),
         ...state.slice(idx + 1)
       ]

     case 'DOWNVOTE_QUOTE':
       idx = state.findIndex(quote => quote.id === action.quoteId)
       quote = state[idx]
 
       if(quote.votes > 0){
       return [
         ...state.slice(0, idx),
         Object.assign({}, quote, {votes: quote.votes -= 1}),
         ...state.slice(idx + 1)
 
       ]
     }
     return state
 
   default: 
   return state; 
   }
}

export default quoteReducer
```

