To **avoid** having to put to Many elements in the DOM we use **fragments** like such:

```jsx
class ChildComponent extends Component {
  render() {
    //The wrapping 'div' here has been replaced with a React fragment
    return (
      <> //  <- to the left is a fragment
        <p>Hey, I am a child</p>
        <p>My name is child component</p>
      </>
    )
  }
}
 
class ParentComponent extends Component {
  render() {
    return (
      <div>
        <ChildComponent />
        <ChildComponent />
      </div>
    )
  }
}
```

The `<>` and `</>` are shorthand for `<React.Fragment>` and `</React.Fragment>` and can be used interchangeably. **They allow a component to return multiple elements without adding a wrapper element that adds to the DOM.**

**Imagine** you had an **array of book** objects in your props that you want rendered to the DOM. Each book has multiple attributes you want to display, but you don't need an element that wraps around these attributes. A fragment can be used here, and can still take a key attribute:

```jsx
const Bookshelf = props => {
  return (
    <section>
      {props.books.map(book => (
        <React.Fragment key={item.id}>
          <h1>{book.title}</h1>
          <h2>{book.author}</h2>
        </React.Fragment>
      ))}
    </section>
  );
}
```