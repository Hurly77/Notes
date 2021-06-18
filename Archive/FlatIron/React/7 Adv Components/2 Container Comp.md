**Container Components** are almost always “parents” of presentational components

Here's a concise **definition** of the container component pattern:

- Container components are primarily concerned with *how things work*
- They rarely have any HTML markup of their own, aside from a wrapping `<div>`;
- They are often stateful
- They are responsible for providing data and behavior to their children (usually presentational components).

## An Example

`BookList` widget component,  the component should just render the current user's book list. Here's one way this component could be written:

```jsx
class BookList extends Component {
  constructor(props) {
    super(props);
 
    this.state = {
      books: []
    };
  }
 
  componentDidMount() {
    fetch('https://learn-co-curriculum.github.io/books-json-example-api/books.json')
      .then(response => response.json())
      .then(bookData => this.setState({ books: bookData.books }))
  }
 
  renderBooks = () => {
    return this.state.books.map(book => {
      return (
        <div className="book">
          <img src={ book.img_url } />
          <h3>{ book.title }</h3>
        </div>
      )
    })
  }
 
  render() {
    return (
      <div className="book-list">
        { this.renderBooks() }
      </div>
    )
  }
}
```

- It assumes that the call to the api returns a JSON object containing a list of book objects with the properties `img_url` and `title`.
- It assumes that the book list will always be rendered with the same markup returned by the render function.

what if the data returned by our API call changed at some point in the future? Suddenly we'd have errors. Or, what if we found that we needed book lists elsewhere in our app? Could we reuse this code? No! Because the whole thing has too many assumptions built-in; it's just too opinionated about how it *should* be used. 

## Separating Concerns Using a Container Component

we can **break** the above single **component** into **two** components. First, we'd isolate the UI layer into a presentational component; then we'd wrap that presentational component in a container component that handles the state and other business logic.

**First**, we have the presentational component, `Book`:

```jsx
// src/Book.js
import React from 'react';
 
const Book = ({ title, img_url }) => (
  <div className="book">
    <img src={ img_url } alt={title}/>
    <h3>{ title }</h3>
  </div>
)
 
export default Book;
```

The component above has one thing to do - given props of `title` and `img_url`, define how these props should be displayed

**one level up** from `Books`, we can write a second component, `BookList`. This component takes in a `books` prop and maps over it, rendering a `Book` component for each element in the `books` prop.

```jsx
// src/BookList.js
import React from 'react';
import Book from './Book';
 
const BookList = ({ books }) => (
  <div className="book-list">
    { books.map(book => <Book title={book.title} img_url={book.img_url} />) }
  </div>
)
 
export default BookList;
```

there isn't really anything complicated going on - this component could also be considered a presentational component, as all it really does is to define how to display the `Book` components and to pass data to those components.

the **final component**, where the main logic is:

```jsx
// src/BookListContainer.js
import React from 'react';
import BookList from './BookList';
 
class BookListContainer extends React.Component {
  constructor() {
    super()
 
    this.state = {
      books: []
    };
  }
 
  componentDidMount() {
    fetch('https://learn-co-curriculum.github.io/books-json-example-api/books.json')
      .then(response => response.json())
      .then(bookData => this.setState({ books: bookData.books }))
  }
 
  render() {
    return <BookList books={this.state.books} />
  }
}
 
export default BookListContainer;
```

This component maintains the state and handles a request for data from a remote API. In terms of rendering, all it does it render `BookList` and pass a piece of state down.

We could, for example, write another container component — let's say `FavoritedBookListContainer` — and then import and wrap `BookList` in order to build a different piece of our UI with the same code.

## Addendum on Container Components

React is an ever evolving framework. Using container and presentational components is just one way in which we can write and structure React applications. New additions to the language, such as [React hooks](https://reactjs.org/docs/hooks-intro.html), solve some of the challenges that were helped by creating separate components to handle logic and presentation. The originator of the container design pattern, Dan Abramov (part of the React dev team), has added a note to his original post on container components regarding this.

While container components have been de-emphasized, they are still a useful tool while you get your feet wet with React. Structuring apps with container and presentational components helps us to understand the way data is shared in an application. It allows us to think in a more object-oriented way about React components and can keep our applications organized.

It is now entirely possible to write React applications in a single functional component, and we encourage you to explore the newest features of React after completing these lessons.

## Resources

- [Container Components](https://medium.com/@learnreact/container-components-c0e67432e005#.2kd1wuyp4)
- [CSS Tricks: Container Components](https://css-tricks.com/learning-react-container-components/)