## Deeper explanation of mapStateToProps

In the current codebase, we have the code:

```jsx
// ./src/App.js
...
 
connect(mapStateToProps)(App)
```

We want to connect our **App**  component to a slice of the store’s state, this is specified in `mapStateToProps()`. Currently our `mapStateToProps()` looks like the following:

```jsx
const mapStateToProps = (state) => {
  debugger;
  return { items: state.items }
}
```

If we boot up the app and check the console we can see that even though we are not updating our **App** component with information about users, the **IMPORTANT**: `mapStateToProps()` function is executed with each change to the store’s state. 

Ok, now the next time we are in the debugger, let's notice that if you type the word state into the console while inside the `mapStateToProps()` method, that it is the entire state of the store and not just that relevant to the component.

Next question: what is so special about this `mapStateToProps()` method that it is executed each time there is a change in state, and receives the entire state of the store as its argument? Let's change our code to the following in `src/App.js`