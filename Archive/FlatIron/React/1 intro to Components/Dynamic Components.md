# **Dynamic Components**

components:

- `BlogContent` - contains the content of the blog post
- `Comment` - contains one user's comment
- `BlogPost` - the 'top level' React component, which is responsible for rendering both `BlogContent` and `Comment`

##### `BlogContent`

The following snippet shows how we can describe variables in our components' `render()` methods:

```JSX
class BlogContent extends React.Component {
  render() {
    return (
      <div>
        {this.props.articleText}
      </div>  
    )
  }
}
```

This line is telling React to place the value that `this.props.articleText` represents within the `<div>`. Ok, so where does `this.props.articleText` come from?

#### Passing Information

React allows us to pass units of information from a parent component down to a child component. We call these **props** we can pass information from `BlogPost` down to its child `BlogContent`:

```jsx
class BlogPost extends React.Component {
  render() {
    return (
      <div>
        <BlogContent articleText={"Dear Reader: Bjarne Stroustrup has the perfect lecture oration."}/>
      </div>
    )
  }
}
```

when we render the `BlogContent` component, we also create a prop called `articleText` that we assign a value of "Dear Reader: Bjarne Stroustrup has the perfect lecture oration." This value is accessible from within the `BlogContent` component as `this.props.articleText`! To create props, we write them the same way as writting attributes for an HTML tag. But remember, this is JSX and not HTML!

> One more thing about props: they can be **any data type!** In our example, we pass a string as a prop. But we can pass a number, boolean, object, function, etc. as a prop!

#### Expanding our Application

##### `Comment`

```jsx
class Comment extends React.Component {
  render() {
    return (
      <div>
        {this.props.commentText}
      </div>
    )
  }
}
```

This component, when used, will display content that is passed down to it, we can make as many as we want:

```jsx
class BlogPost extends React.Component {
  render() {
    return (
      <div>
        <BlogContent articleText={"Dear Reader: Bjarne Stroustrup has the perfect lecture oration."}/>
        <Comment />
        <Comment />
        <Comment />
      </div>
    )
  }
}
```

..and just as before, we can pass content data down to them:

```jsx
class BlogPost extends React.Component {
  render() {
    return (
      <div>
        <BlogContent articleText={"Dear Reader: Bjarne Stroustrup has the perfect lecture oration."}/>
        <Comment commentText={"I agree with this statement. - Angela Merkel"}/>
        <Comment commentText={"A universal truth. - Noam Chomsky"}/>
        <Comment commentText={"Truth is singular. Its ‘versions’ are mistruths. - Sonmi-451"}/>
      </div>
    )
  }
}
```

we are passing information from a parent component to many child components. Specifically, **we are** doing this by **creating a prop called** `commentText` to pass to each `Comment` component, which is then accessible in each instance of `Comment` as `this.props.commentText`

**React components:**

- are modular, reusable, and enable a 'templating' like functionality
- help us organize our user interface's *logic* and *presentation*
- enable us to think about each piece in isolation, enabling us to apply structure to complex programs

# lab

#### `Comment` Component

- Create a  `Comment` component in the file, `Comment.js` within `src/` and don't forget to:
- `import React, { Component } from 'react'` at the top of our file
  - Use the `class X extends Component {}` syntax
- export the class so it can be used in other files
  - import the class in `BlogPost`
- It should expect a single prop (the text of a comment), which can be used in the component via: `this.props.commentText`. This prop is passed in `src/BlogPost.js`
- It should have a single `<div>` in its `render()` method
- The `<div>` should have a `className="comment"` attribute
- **Note:** The `BlogPost` component needs *minor* alteration to properly pass the contents of its `commentsArray` to each of the `Comment` components that it is rendering
- Don't forget - we can unpack variable values directly with JSX by wrapping them in `{}`, i.e. `{this.props.commentText}`

```jsx
import React, {Component} from 'react'

export default class Comment extends Component {
  render() {
    return (
      <div className="comment">
        {this.props.commentText}
      </div>
    )
  }
}
```



#### `ColorBox` Component

- Should expect a single prop (an opacity value), which can be used in the component via: `this.props.opacity`. This prop is first passed in `src/App.js`

- If the opacity value

   

  is greater than or equal to 0.2

  :

  - the `ColorBox` component should render another `ColorBox` inside itself (recursive components!)
  - an opacity prop should be passed to the child
  - the passed opacity prop should be reduced by 0.1

- If the opacity value

   

  is less than 0.2

  :

  - do not render another `ColorBox` (or else we would have infinite `ColorBoxes` rendering!)
  - instead, the render method should return `null`

```jsx
import React, { Component } from 'react';

export default class ColorBox extends Component {

  state = {
    todos: [
      
    ]
  }

  render() {
    let i = this.props.opacity
    let c = <ColorBox opacity={this.props.opacity - 0.1}/>
    return (
      <div className="color-box" style={{opacity: this.props.opacity}}>
        {i > 0.2 ? c : null}
      </div>
    )
  }

}
```

