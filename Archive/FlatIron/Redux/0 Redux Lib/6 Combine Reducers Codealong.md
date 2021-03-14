We have learned about reducers when working with one resource however on a larger scale we need to be working with multiple resources at a time that is where the `combineReducers()` function comes in. This function will allow us to delegate different pieces of state to separate reducer functions.

**we’ll build an app** that does two things 

1. Keep track of each book that has been read. `title`, `author`, `description`.
2. keep track of the authors who wrote these books.

#### Determine Application State Structure

**Our app** will need a state object the stores two types of info

1. all books, in an array
2. Our authors, also in an array.

Each type of info should be represented in our **store’s state object**. Think of **store’s state** structure as a database. We can represent this as a belongs to/has many relationship, in that a book belongs to an author and an author has many books.

set application state.

```jsx
{
  authors: //array of authors
  books: // array of books,
}
```

Our State object will have two top-level **keys**. both will point to an **array**. lets write a **single** reducer for both of these resources.

```jsx
//src/reducers/manageAuthorsAndBooks.js,
export default function bookApp(
  state = {
    authors: [],
    books: []
  },
  action
) {
  let idx;
  switch (action.type) {
    case "ADD_BOOK":
      return {
        ...state,
        authors: [...state.authors],
        books: [...state.books, action.book]
      };
 
    case "REMOVE_BOOK":
      idx = state.books.findIndex(book => book.id === action.id);
      return {
        ...state,
        authors: [...state.authors],
        books: [...state.books.slice(0, idx), ...state.books.slice(idx + 1)]
      };
 
    case "ADD_AUTHOR":
      return {
        ...state,
        books: [...state.books],
        authors: [...state.authors, action.author]
      };
 
    case "REMOVE_AUTHOR":
      idx = state.authors.findIndex(author => author.id === action.id);
      return {
        ...state,
        books: [...state.books],
        authors: [...state.authors.slice(0, idx), ...state.authors.slice(idx + 1)]
      };
 
    default:
      return state;
  }
}
```

> **Immutable state** - Is a state that remains unchanged, or is not subject to change over time in an arbitrary fashion.

By adding two recourses to our reducers our code size increased plus, we’d ideal like our concerns to be separated.  

The reason we use the spread operator on both the books and author resources is beacuse if we just did the following

```
return {
        ...state,
        authors: [...state.authors, action.author]
};
```

a new reference to the old `state.books` array will be used, not a *new copy of the array*. We want to maintain a immutable state

## Refactor by using combineReducers

The `combineReducers()` function will allow us to write separate reducers and combine them to produce the above result. Then **pass that combine reducer to ** the store in `src/index.js`. 

```js
import {combineReducers} from 'redux';

const rootReducer = combineReducers({
    authors: authorsReducer, 
    books: booksReducer
})

export default rootReducer;// 

function booksReducer(state = [], action) {
  let idx;
  switch (action.type) {
    case "ADD_BOOK":
      return [...state, action.book];
 
    case "REMOVE_BOOK":
      idx = state.findIndex(book => book.id  === action.id)
      return [...state.slice(0, idx), ...state.slice(idx + 1)];
 
    default:
      return state;
  }
}
 
function authorsReducer(state = [], action) {
  let idx;
  switch (action.type) {
    case "ADD_AUTHOR":
      return [...state, action.author];
 
    case "REMOVE_AUTHOR":
      idx = state.findIndex(author => author.id  === action.id)
      return [...state.slice(0, idx), ...state.slice(idx + 1)];
 
    default:
      return state;
  }
}
```

In the top part of our code 

```js
import { combineReducers } from "redux";
 
const rootReducer = combineReducers({
  authors: authorsReducer,
  books: booksReducer
});
 
export default rootReducer;
```

we Import **{combineReducers}** from Redux then we write a variable `rootReducer` and pass in an **object** to `combineReducers` function whose **keys** are the **state** to be reduced and **values** which are that of the return of the **function** they are pointing to.

Since we've changed the default export of `manageAuthorsAndBooks.js`, in `index.js`, we don't need to change anything with createStore unless we wanted to update names we've assigned:

```jsx
import { createStore } from "redux";
import rootReducer from "./reducers/manageAuthorsAndBooks";
 
const store = createStore(
  rootReducer,
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
);
```

By passing our rootReducer to the createStore method, the application maintains its initial state of `{ books: [], authors: [] }`, just as it did when we had one reducer. From the application's perspective nothing has changed.

#### Examining Our New Reducers

Looking at the `authorReducer` authors on concerns itself with it’s slice of state. Also, we no longer need to make a call to `state.authors` we can just call **state**:

```jsx
function authorsReducer(state = [], action) {
  let idx;
  switch (action.type) {
    case "ADD_AUTHOR":
      return [...state, action.author];
 
    case "REMOVE_AUTHOR":
      idx = state.findIndex(author => author.id === action.id);
      return [...state.slice(0, idx), ...state.slice(idx + 1)];
 
    default:
      return state;
  }

```

#### Dispatching Actions

The `combineReducer()` function returns to us one large reducer that looks like the following:

```jsx
function reducer(state = {
  authors: [],
  books: []
}, action) {
  let idx
  switch (action.type) {
 
    case "ADD_AUTHOR":
      return [...state, action.author]
 
    case 'REMOVE_AUTHOR':
      ...
  }
}
```

This allows us to dispatch actions the like we always have. `store.dispatch({ type: 'ADD_BOOK', { title: 'Snow Crash', author: 'Neal Stephenson' } });` will hit our switch statement in the reducer and add a new author.

if you want to have more than one reducer respond to the same action, you can.

For example, in our application, when a user inputs information about a book, the user *also* inputs the author's name. It would be handy if, when a user submits a book with an author, that author is also added to our author array.

The action dispatched doesn't change: `store.dispatch({ type: 'ADD_BOOK', { title: 'Snow Crash', author: 'Neal Stephenson' } });`. Our `booksReducer` can stay the same for now:

```JS
function booksReducer(state = [], action) {
  let idx;
  switch (action.type) {
    case "ADD_BOOK":
      return [...state, action.book];
 
    case "REMOVE_BOOK":
      idx = state.findIndex(book => book.id === action.id);
      return [...state.slice(0, idx), ...state.slice(idx + 1)];
 
    default:
      return state;
  }
}
```

However, in `authorsReducer`, we can *also* include a switch case for "ADD_BOOK":

```JSX
import uuid from "uuid";
 
function authorsReducer(state = [], action) {
  let idx;
  switch (action.type) {
    case "ADD_AUTHOR":
      return [...state, action.author];
 
    case "REMOVE_AUTHOR":
      idx = state.findIndex(book => book.id === action.id);
      return [...state.slice(0, idx), ...state.slice(idx + 1)];
 
    case "ADD_BOOK"://CODE CHANGE		
      let existingAuthor = state.filter(
        author => author.authorName === action.book.authorName
      );
      if (existingAuthor.length > 0) {
        return state;
      } else {
        return [...state, { authorName: action.book.authorName, id: uuid() }];
      }
 
    default:
      return state;
  }
}
```

