## What makes a component "presentational"?

To put it simply the only job a **presentational component** has is to look good. though there is *no such thing as a *presentational Component* in the React Library, there is a *convention* or, better put a ***programming** **pattern***.

**What defines** a presentational component

- Presentational components are primarily concerned with how things look
- If they are class components, they probably only contain a render method. If functional, they just return some JSX
- They do not know how to alter the data that they render
- They rarely have any internally changeable `state` properties
- They are best written as stateless functional components

## We’ve already written presentational components

Remember a presentational component is simply just a component the does not know anything about how to het data it displays.

```jsx
class HelloWorld extends Component {
  render() {
    return <div className="hello-world">Hello {this.props.message || 'World' }</div>;
  }
}
```

This component does nothing but render a piece of our UI; that has no notion of how to fetch or reload the `message` data that it takes in as a `prop`; that has no changeable state; and that only contains a render method.

## when would we need presentational components

**consider this**: let's say we are working on a massive web application, and we'd like to standardize as well as place some limits on the characteristics of the text inputs used throughout the application's forms.

**Instead of using style guides**, which would be prone to human error and if we used any props we’d leave the type of props that can be provided wide open. 

  Consider this `TextField` component:

```jsx
const defaultLimit = 100
 
class TextField extends Component {
  render() {
    return (
      <input
        className="field field-light"
        onChange={this.props.onChange}
        maxLength={this.props.limit || defaultLimit}
      />
    );
  }
}
```

This component fits presentational pattern and it is really powerful as well. this is just a simple wrapper but, it creates CSS classes in one place for all the input fields used throughout the app.

## The "Stateless Functional" Component & "Pure" Functions

We can make or `TextField` even simpler

`TextField` component rendered as a so-called "functional stateless" component (a feature available in React since v0.14):

```jsx
const defaultLimit = 100
 
const TextField = (props) =>
  <input
    className="field field-light"
    onChange={props.onChange}
    limit={props.limit || defaultLimit}
  />;
```

we have made our `TextField` component an extremely stable and predictable part of our application.

The predictability comes from the fact -- and here we can see the influence of the principles of functional programming on React -- that this function will always return the same UI output if given the same `props` what in functional terms is called a "**pure**" or **"referentially transparent"** function.