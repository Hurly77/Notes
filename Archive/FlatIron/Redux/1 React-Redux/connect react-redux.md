# **When To Connect React and Redux**

## Component State vs Store State

We don’t always have to move state out of components. sometimes it makes things even more complex. Using `setsState()` and “local” component-level state is a perfectly fine choice in most cases.

**In genera**l, we should not start out by putting all our state into some form of global store (or multiple stores).

**When architecting** a user interface(**UI**), try to use local state and parent props **first**. If we end up constantly passing down tons of props, we should consider connecting the component in question with a Redux store.

**E.g**. let's say we want to render some form of carousel, something like [Bootstrap's Carousel component](https://getbootstrap.com/docs/4.0/components/carousel/).

A carousel is a perfect example on where using a store to extract out component state doesn't necessarily make things easier (or would simply be a massive overkill).

Writing the essential handler functions for the component in question using "classical" React-style without **any** "outside" state is trivial:

```jsx
class Carousel extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      // We start out rendering the first slide. 0 denotes the index of the
      // active item.
      currentSlide: 0,
    };
  }
  /**
   * Handler function that transitions to the next slide in the carousel.
   * This is the function that will be run once the user clicks the "next"
   * button.
   */
  goNext = () => {
    const previousSlide = this.state.currentSlide;
    this.setState({ currentSlide: previousSlide + 1 });
  }
  /**
   * Equivalent to `goNext`. Handler function that will be invoked when clicking
   * the "back" button.
   */
  goBack = () => {
    const previousSlide = this.state.currentSlide;
    this.setState({ currentSlide: previousSlide - 1 });
  }
  render() {
    // Magic goes here
  }
}
```

In this case, using the local state of the component has a couple of advantages over using an external store:

1. The state is **by definition** bound to the component

> When rendering a very long list of carousels, keeping the state stored in the store in sync with the *actual* list of rendered components is hard. Let's say we render one carousel for each photo "collection" — which could for example be represented by an array for image sources — keeping the array length in sync with whatever data structure we would use in the store for representing the selected slide index is unnecessarily complex. For example, when adding a photo collection, we would need to add the `currentSlide` property to the store as well.
>
> Simply distinguishing between "component UI" state and global application state radically simplified the architecture in the above case, since component state can by definition not exist without a matching component (and vice versa).

2. Simplified Testing

> Testing React components is extremely easy compared to other frameworks, such as Angular. Testing packages like [Enzyme](https://airbnb.io/enzyme/) from Airbnb allow us to mount individual components in a test, pass them props, cause state changes, check what JSX is rendered, etc...
>
> Using stores doesn't necessarily break this abstraction, but it makes it much harder to properly test all the possible states that a component can be in, since a store might contain state that isn't directly consumed by the component to be tested.
>
> But more importantly, we now need to manage a store during testing. We can use the same packages and functions like `createStore()` we use to set up Redux with React, but the tests become more complicated and sometimes less flexible as a result.
>
> We can also mock it out — some node packages allow us to create a fake store for the tests. Overall, though, because Redux changes the way data is maintained, tests need to change accordingly, becoming more complicated.

3. Reusing the component is possible

> While we focused on implementing our own set of stores, some people prefer to use Redux, Rx, mobx or some other library for managing state and implementing unidirectional data flow.
>
> By storing state in an external store, we implicitly couple the component to whatever architecture we chose for our main application. If we're implementing an accordion component using [Flux](https://facebook.github.io/flux/) (the data flow pattern Redux is based on), it means everyone using our component will have to use Flux in order to interact with it (even though it might be hidden through the public API of the component).
>
> Hence using component state (and props) instead of stores is the preferred way when creating reusable components.

## Presentational vs Container Components

While it is possible to connect any component to our store, one pattern is to only connect **Container components**. Since they are primarily concerned managing state and actions that mutate the state of an app, they tend to be a good place to connect to the store.

In single page apps, a good rule of thumb is to make each page of your application (or component attached to a sub-route) a container component. While it isn't necessarily a bad idea to use nested container components, passing props to pure components tends to be easier to test and reason about.

**Presentational components** are modular, reusable (and typically small) components that are concerned with "how stuff looks". In this pattern, they are not typically connected to a store.

Usually UI elements (with a bit of interaction) are presentational components and therefore not concerned with the actual state of the application. E.g. a modal, accordion, or button should not be container components. They only receive props and display them.

The benefit of separating presentational components from any **Redux** specific code is that it makes them highly reusable. They're just React components, after all.

This is only a suggested pattern. Deciding whether or not something should be a container or presentational component is not a definitive decision. Making presentational components stateful by wiring them up to a store is usually quite easy and gets rid of a lot of indirection. For example passing down a lot of different props 5 levels deep is much more error prone than simply connecting the "leaf" component to the store.

It also means we don't need to re-render all the components in between the presentational leaf component that is due to be rendered and the intermediate components that simply pass down the state via props from the container component.