## Use`document.querySelector()` and `document.querySelectorAll()` to Find Nested Nodes

### `querySelector()`

Given a document like

```html
<body>
  <div>
    Hello!
  </div>
 
  <div>
    Goodbye!
  </div>
</body>
```

`document.querySelector('div')`, the method would return the first `div` (whose content is "Hello!").

We can get very specific.

```html
>
    Hello!
  </div>
 
  <div>
    Goodbye!
  </div>
</body>
If we called document.querySelector('div'), the method would return the first div (whose content is "Hello!").

Selectors aren't limited to one tag name, though (otherwise why not just use document.getElementsByTagName('div')[0]?). We can get very specific.

<body>
  <div>
    <ul class="ranked-list">
      <li>1</li>
      <li>
        <div>
          <ul>
            <li>2</li>
          </ul>
        </div>
      </li>
      <li>3</li>
    </ul>
  </div>
 
  <div>
    <ul class="unranked-list">
      <li>6</li>
      <li>2</li>
      <li>
        <div>4</div>
      </li>
    </ul>
  </div>
</body>
```

```javascript

// get <li>2</li>
const li2 = document.querySelector('ul.ranked-list li ul li')
 
// get <div>4</div>
const div4 = document.querySelector('ul.unranked-list li div')
```

we've called `querySelector()` on), find a `ul` with a `className` of `ranked-list` (the `.` is for `className`).

*NOTE: The HTML property `class` is referred to as `className` in JavaScript.*

### `querySelectorAll()`

`uerySelector()` — it accepts a selector as its argument, and it searches starting from the element that it's called on (or from `document`) — but instead of returning the first match, it returns a `NodeList` collection (which, remember, is not *technically* an `Array`) of all matching elements.

```html
<main id="app">
  <ul class="ranked-list">
    <li>1</li>
    <li>2</li>
  </ul>
 
  <ul class="ranked-list">
    <li>10</li>
    <li>11</li>
  </ul>
</main>
```

If we called

```javascript
document.getElementById('app').querySelectorAll('ul.ranked-list li')
```

We'd get back a list of Nodes corresponding to: `<li>1</li>, <li>2</li>, <li>10</li>, <li>11</li>`.

## **Removing, Altering and Inserting HTML Lab**

## Add Elements in the DOM

```javascript
let element = document.createElement('div')
//undefined
```

### `appendChild()`

 appends `element` to `body` to start:

```javascript
document.body.appendChild(element)
```

We can continue to update `element`, since we have a reference to it:

```javascript
let ul = document.createElement('ul')
 
for (let i = 0; i < 3; i++) {
  let li = document.createElement('li')
  li.innerHTML = (i + 1).toString()
  ul.appendChild(li)
}
 
element.appendChild(ul)
```

## Add Elements to the DOM via `innerHTML`

there's another route which is commonly used, `Element.innerHTML`. If you can get a node with `getElementById` or `querySelector` or any of the modes you've learned, you can imagine that you've gotten that node's opening and closing HTML tag. You can update that node's `innerHTML` property with a string of HTML and it will be *just as if* you changed the HTML source for that node.

```javascript
let element = document.querySelector("p#greeting");
element.innerHTML = 'Hello, DOM!'
```

```javascript
let header = document.getElementById("div#header");
header.innerHTML = "<h1>Poodles!</h1><h3>An Essay into the Pom-Pom as Aesthetic Reconfiguration of the Other from a post-Frankfurt School Appropriationist Perspective</h3><p><em>By: Byron Q. Poodle, Esq., BA.</em></p>";
```

This Creates with JavaScript, in the DOM the quivalent of :

```html
<div id="header">
  <h1>Poodles!</h1>
  <h3>An Essay into the Pom-Pom as Aesthetic Reconfiguration of the Other from a post-Frankfurt School Appropriationist Perspective</h3>
  <p><em>By: Byron Q. Poodle, Esq., BA.</em></p>
</div>
```

## Change Properties on DOM Nodes

```javascript
element.style.backgroundColor = '#27647B';
```

```javascript
element.style.textAlign = 'center';
ul.style.textAlign = 'left'
```

 JavaScript and update its `className` property — which is the same as setting the `class` property in the HTML. The `className` property expects a `String` where each class name is separated by a space:

```javascript
element.className = "dog"
element.className = "pet-listing dog"
```

Sometimes it's easier to add classes programmatically, instead of creating a long `String` first. JavaScript makes this friendly by having elements provide a `classList` [property](https://developer.mozilla.org/en-US/docs/Web/API/Element/classList) which has `.add()` and `.remove()` methods.

```javascript
element.classList.remove("this-is-fine");
element.classList.add("the-room-is-on-fire");
```

Why go through the trouble of defining appearance in a stylesheet which is applied by [classList](https://developer.mozilla.org/en-US/docs/Web/API/Element/classList) versus simply using JavaScript to change the appearance? Again, this goes back to a fundamental programming concept about separating concerns between technologies:

- HTML defines the structure of the website (not appearance or functionality)
- JavaScript defines functionality of the website (not structure or styling)
- CSS defines the visualization and style of the website (not structure or functionality)

## Remove Elements from the DOM

#### `removeChild()`

`removeChild()` method requires us to find a parent and tell it to remove its already-found child:

```javascript
ul.removeChild(ul.querySelector('li:nth-child(2)'))
```

The second element is gone!

What if we want to remove the whole unordered list (`ul`)?

### `element.remove()`

We can just call `remove()` on the element itself:

```javascript
ul.remove()
```

## Lab

```javascript
document.getElementById("main").remove();

let newHeader = document.createElement('h1');
newHeader.setAttribute("id", "victory");
newHeader.innerHTML = "YOUR-NAME is the champion"
```

[`newHeader.setAttribute("id", "victory");`](https://stackoverflow.com/questions/9422974/createelement-with-id)

