# **React Components**

we want an application that displays an opinion and responses to that opinion (like a 'comments' section).

#### Step 1 -- write the components

First, let's make a component to showcase an opinion:

```jsx
class Article extends React.Component {
  render() {
    return (
      <div>
        Dear Reader: Bjarne Stroustrup has the perfect lecture oration.
      </div>
    )
  }
}
```

 let's make a component to display a single user's comment:

```jsx
class Comment extends React.Component {
  render() {
    return (
      <div>
        Naturally, I agree with this article.
      </div>
    )
  }
}
```

#### Step 2 -- use the components

we need to do is make sure some other component is making use of them in it's `render` method. Every React application has some top level component(s). Very often, this **top level component is simply called** `App`.

```jsx
class App extends React.Component {
  render() {
    return (
      <div>
        <Article /> <!--this is JSX syntax-->
        <Comment />
      </div>
    )
  }
}
```

What we are seeing in this `App` component's `render()` method is a straightforward description of what we want HTML will look like:

```html
<div>
  <div>Dear Reader: Bjarne Stroustrup has the perfect lecture oration.</div>
  <div>Naturally, I agree with this article.</div>
</div>
```

This unpacks logically. The `App` component (being our top level component) wraps around both `Article` and `Comment`, and we already know what they look like when they are turned into HTML.

# lab

- Two components have not been properly imported in `src/App.js`. Identify and debug the issue. The stack trace when running `npm test` should point you in the right direction! **HINT**: take a look at the component files. One of these components is exported by `default`, but the other is *not*. How does this change importing?
- Once you have the first two components importing correctly, import and render a third component, `MouseComponent`. In total, `App` needs to render three children components:

1. `CatComponent`
2. `GraceHopperQuoteComponent`
3. `MouseComponent`

```jsx
import React, { Component } from 'react';
import CatComponent from './CatComponent'
import GraceHopperQuoteComponent from './GraceHopperQuoteComponent'
import MouseComponent from './MouseComponent'
//had to import the last three

class App extends Component {
	render() {
		// your code in the return statement below!
		return (
			<div className="App">
				<CatComponent />
				<GraceHopperQuoteComponent />
				<MouseComponent />
                <!--needed to use MouseComponent Tag-->
				{/* one more component missing */}
			</div>
		);
	}
}

export default App;
-----------------------------------------------------------------------
import React, { Component } from 'react';

//default was missin from most classes
export default class CatComponent extends Component {
  render() {
    return (
      <div className="bar" id="cat">
        <img src="/cat.gif" />
      </div>
    );
  }
}
```

