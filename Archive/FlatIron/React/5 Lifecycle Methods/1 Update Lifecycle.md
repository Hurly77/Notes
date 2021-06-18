We are kindly provided with 4 lifecycle methods to help us handle updates:`static getDerivedStateFromProps()`, `shouldComponentUpdate`, `getSnapshotBeforeUpdate` and `componentDidUpdate`.

These methods always get called in the same order and the `render()` method which renders the React component into the DOM will be called just before `componentDidUpdate`, so the actual order of lifecycle methods being called is:

1. `static getDerivedStateFromProps(props, state)`
2. `shouldComponentUpdate(nextProps, nextState)`
3. `render()` (can access props and state via `this.props` and `this.state` - previous props are no longer available)
4. `getSnapshotBeforeUpdate(prevProps, prevState)`
5. `componentDidUpdate(prevProps, prevState, snapshot)` (can still access current props and state via `this.props` and `this.state` and this is the last time previous props and state will be available).

#### `static getDerivedStateFromProps(props, state)`

this method is called on *every* update, whenever the component is receiving new props from parent or the component stat has changed. 

**A word of caution**: a **common mistake** here is to assume that the props have changed. Just because the method is called doesn't necessarily mean that the props have changed. It is entirely possible that a parent component has updated and in re-rendering has passed the *same* props down to its children. In this case, regular components will still be triggered to update.

**THIS METHOD** is usually **not** **needed**, and in many cases where it seems necessary, there is often a better solution.

#### `shouldComponentUpdate(nextProps, nextState)`

The `shouldComponentUpdate` method doesn't operate on the state, but has a `Boolean` return value determining whether the component should update or not. **for instance**, you only want a component to update when a value changes past a set threshold, you could use this method to prevent component updating *until* the props meet the requirement.

**React recommends** an **alternative** - use `React.PureComponent` instead of `React.Component`. From the [React reference materials](https://reactjs.org/docs/react-api.html#reactpurecomponent):

> "**If your React component’s render()** function renders the same result given the same props and state, you can use React.PureComponent for a performance boost in some cases."

The `PureComponent` **does not have access to** `shouldComponentUpdate`, because it instead runs its own version. The `PureComponent` checks to see if there are any *shallow* changes to props and state and will only update if it registers a difference between the current and next states.

to customize the logic for when to re-render, use `shouldComponentUpdate`.

```jsx
shouldComponentUpdate(nextProps, nextState) {
  return (this.props.myImportantValue !== nextProps.myImportantValue);
}
```

#### `render()`

At this stage, the next props and state have become available as `this.props` and `this.state` and the component gets rendered into React's virtual DOM.

#### `getSnapshotBeforeUpdate(prevProps, prevState)`

Right after render, but *just before* React commits content from its virtual DOM to the actual DOM, the `getSnapshotBeforeUpdate` method is fired. 

his method is **currently only used** to capture information that may be changed after an update. **For instance**, mouse position and scroll position might be changing rapidly and will change by the time the next lifecycle method is invoked. This method **returns** either `null` or a value that will be passed into the next method, `componentDidUpdate`.

#### `componentDidUpdate(prevProps, prevState, snapshot)`

This method **isn't used very often** but it is kind of a look back to the update that just occurred. We will have access to both the current props and previous props, as well as any snapshot info from `getSnapshotBeforeUpdate`. A common use case for this would be to update a 3rd party library.

```jsx
 componentDidUpdate(previousProps, previousState) {
   if (previousProps.height !== this.props.height) {
     someChartLibrary.updateHeight(this.props.height);
   }
  }
```

#### Updating lifecycle methods

Not called on initial render, but always called whenever a subsequent re-render is triggered:

|               Method                | current props and state | previousProps | previousState | nextProps | nextState | Can call `this.setState` |                         Called when?                         |                           Used for                           |
| :---------------------------------: | :---------------------: | :-----------: | :-----------: | :-------: | :-------: | :----------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| `static getDerivedStateFromProps()` |           yes           |      no       |      no       |    no     |    no     |           yes            |                     before every render                      |                        Not used often                        |
|       `shouldComponentUpdate`       |           yes           |      no       |      no       |    yes    |    yes    |           yes            |            before every re-render (not initially)            | can be used to stop unnecessary re-renders for performance optimization |
|      `getSnapshotBeforeUpdate`      |           yes           |      yes      |      yes      |    no     |    no     |           yes            | just before React updates and commits new content to the DOM |  used rarely; can capture data that may be changing rapidly  |
|        `componentDidUpdate`         |           yes           |      yes      |      yes      |    no     |    no     |           yes            |             just after a re-render has finished              | any DOM updates following a render (mostly interacting with 3rd party libraries) |

`componentDidUpdate` will actually receive the previous props and state as arguments, as the newly applied state and props can be accessed through `this.props` and `this.state`.