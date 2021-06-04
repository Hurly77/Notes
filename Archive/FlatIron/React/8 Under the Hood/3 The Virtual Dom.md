## It Is Not a Virtual DOM

The term “**Virtual DOM**” was used to explain how React was able to out preform Traditional **DOM**

**Reconciliation**  - Is the process of React handling updates to the screen

## Updating the DOM

When the DOM updates, the browser recalculates CSS, lays out the DOM tree and ‘repaints’ the display. **The constant** updating of and repainting of the DOM can result in poor performance. 

**However**, with React we have some neat tricks for being smart about it. 

## Reconciliation, Briefly

In React, we use **JSX Elements** to represent DOM elements, and when rendered, become those elements on a webpage.

**Current tree** - During initial render, React uses these elements to build a ‘tree’  to *represent* what the DOM currently looks like.

**WorkInProgress tree**  - when updates could cause re-render in React, a second tree is made, this represents what the DOM *will* look like. Then **WorkInProgress** is use to update **current** tree.

### Grouped Updates

Updates can be grouped together. By waiting until all updates are processed before committing the **workInProgress** tree to the DOM, excessive repaints are avoided.

 **for instance,** you have an app with many components, each colored a shade of blue, and a button, that when pressed, turns all those components to red. When that button is pressed, React will put together a tree containing all the components along with their updated properties, *THEN* commit all the changes to the DOM at once. This only requires one repaint. Without this design, we could end up with code that updates the DOM for each individual part of the app, one repaint for each part.

### Diffing Changes

React can apply a diffing algorithm to quickly see what specific pieces of DOM *need* to be updated and how. This reduces  the number of DOM changes that need to be made and lets React be particular in its updates, improving performance.

In plain JavaScript some DOM changes are better than others in terms of performance. For example, say you want to add something inside a `ul` in your DOM. Using `innerHTML` will work

```jsx
ul.innerHTML += '<li>A final list item</li>'
```

But this *rebuilds* the entire DOM inside `div`. On the other hand, using `appendChild` would *not* cause a rebuild:

```jsx
let li = document.createElement('li')
li.textContent = 'A final list item'
ul.appendChild(li)
```

React's diffing algorithm is designed to identify changes between what the current DOM looks like and what it will look like (the **current** and **workInProgress** trees). Based on the changes it identifies, different DOM updates will be performed to avoid rebuilding unnecessarily.