# How to Modularize Code in VanillaJS & using import export.

**Introduction**
For my next Trick I will be making VanillaJS Modular, and will be using the keywords `import` and `export`. The best part is we will only be using pure HTML, CSS, and JavaScript. No `node` or `npm` Just plain old Vanilla JavaScript.

To get started go ahead and create a new project folder with a file structure like so

```
my-project-name
─────┐
	js/─┐
	 	┊ classes.js
     	┊ selectors.js
	  	┊ main.js
	index.html
	style.css

```

Probably the best part of all this is just how easy it really is to do. Simply just put a script in the `head` of the your `HTML` document. 

Note: **If you're here to just get the answer to "how" copy and paste the below**

```html
<script src="./js/main.js" defer type='module'></script>
```

Now that with that set up we can now use `import` & `export` in files.

## Box creator - code along

To demonstrate how to use `import` & `export` will be creating a box generator, nothing special just some simple styles and some basic code.

First go ahead and set up as style sheet for your `style.css`, do that by adding the following into the head of the document.	

```html
<link rel="stylesheet" href="./style.css">
```

Then I added some some basic styles.

```css
body {
  margin: 0;
  padding: 0;
  background-color: lightblue;
}

* {
  margin: none;
  border: none;
  background-color: none;
}

.container {
  margin: 0;
  height: 100vh;
  width: 100%;
  padding-left: 2px;
}

.form {
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  height: 200px;
  width: 100px;
  margin-left: 10px;
}

.form div {
  display: inherit;
  flex-direction: column;
}
.form label {
  align-self: start;
}

input {
  border: none;
}

input[type='color'] {
  width: 100%;
  padding: 0px;
}

.box-storage {
  display: flex;
  flex-flow: row wrap;
  justify-content: space-evenly;
  width: 100%;
  height: auto;
}
```

Then will add some inputs to our HTML document along with a button to create said box.

```html
 <div class="container">
    <div class="form">
      <div class="item1">
        <label from="box-name">name</label>
        <input type="text" name="box-name">
      </div>

      <div class="item2">
        <label from="color">Color</label>
        <input type="color" name="color">
      </div>

      <div class="item3">
        <label from="height">height</label>
        <input type="number" name="height" name="width" min="10" max="500"
          step="10">
      </div>

      <div class="item4">
        <label from="width">width</label>
        <input type="number" name="width" min="10" max="500" step="10">
      </div>
      <button>Create Box</button>
    </div>

    <div class="box-storage">
		<!-- This is where will put our boxes -->
    </div>
  </div>
```

### Now For The JavaScript

Now that we have some style we can begin to add JavaScript. Inside the  `selectors.js` file will put the following.

```javascript
export const createBox = () => document.createElement('div');
// Note that the reason I've made this a arrow function instead of a variable is so that it won't be using the same div element and will instead create a new one.
export const container = document.querySelector('.container');
export const button = document.querySelector('button');
export const boxName = document.querySelector('input[name="box-name"');
export const boxWidth = document.querySelector('input[name="width"');
export const boxHeight = document.querySelector('input[name="height"');
export const boxColor = document.querySelector('input[type="color"');
export const boxStorage = document.querySelector('.box-storage');
```

Notice how we've used `export` if your not familiar with web pack the keyword describes exactly what it is doing. It is very important to remeber that if you export a variable who's value is not a function the variable will only ever have one instance of itself for example.

```javascript
export const createElm = document.createElment('div')
//this will generate <div></div> and if we where to asign it a class like "box" and then try to use it again only with the a different class say like "box2" we'd only be changing the className not createing a new elment.
```

I like to keep my code as organized as I can and also looking pretty so I decided it would be best to export all selectors from a single file. When projects in VanillaJS get bigger you often will have to use `querySelector` multiple times on the same element so it is good to have a way of only every writting the code once.

Now in our the `classes.js` file lets create a custom JavaScript Object for 'Box' will need to first create and array for all the boxes and `import` the `createBox` function form `selectors.js`.

```JavaScript
import {createBox} from './selector.js'

export let boxes = []
```

**NOTE:** Because we are not using Web Pack we have no easy way of purging our files path. This means that we must be extremly specific as to what the file extension is and where it exactly is in the directory. If we try to import a file like so.

```javascript
import {myFunction} from 'selectors'
```

This will simply just not work.

Next we want to create a `class` for box that will take in a `name`, `color`, `height`, and `width` 

```javascript
export class Box {
 constructor(name, color, h, w) {
  this.id = Math.random()
  this.h = h;
  this.w = w;
  this.name = name;
  this.color = color;
 }
}
```

This will allow us to have multiple instance of the same object. Keep in mind that I'm using the `Math.random()` because there are an infinite possible numbers between zero and one so the likely hood two numbers twice is pretty much null. However this is not a great way of generating an id and would be better to use that `cuid` npm package if you were to actually store this in a DB. However I'm merely using it for demonstration only.

We need to have a way of create a box from the input from our form. So in the `main.js` file lets create a function just for that and add an `eventListener` to our button.

will want to import  the Box class from `classes.js` and all the input selectors, box-storage element and our button from `selectors.js`.

```javascript
import { Box, boxes } from './classes.js';
import {
 boxHeight,
 boxWidth,
 boxColor,
 boxName,
 button,
 boxStorage
} from './selectors.js';
```

Now will create a function for adding a box to the DOM.

```javaScript
function addBox(name, color, h, w) {
  let box = new Box(name, color, h, w);
}
```

and add an event listener that will use this function when button is clicked.

```javascript
button.addEventListener('click', () => {
  addBox(boxName.value, boxColor.value, boxHeight.value, boxWidth.value);
  boxName.value = '';
    //this will remove the name value.
});
```

You'll notice that if we try and test our code nothing will be working and that is because we need to add an instance method to our `Box` class. Back inside our `classes.js` file lets add that following code to `Box` class.

```JavaScript
export class Box {
  ...
  initBox() {
    let htmlBox = createBox();
    //remeber that we are importing createBox() from selectors.js file. Which returns a div element.
    htmlBox.id = this.id;
    htmlBox.style.height = `${this.h}px`;
    htmlBox.style.width = `${this.w}px`;
    htmlBox.style.margin = '6px';
    htmlBox.style.backgroundColor = this.color;
    htmlBox.innerHTML = this.name;
    boxes.push(htmlBox);
  }
}
```

Now Back inside the `main.js` we can use our instance method to initialize the box.

```JavaScript
function addBox(name, color, h, w) {
  let box = new Box(name, color, h, w);
  box.initBox();
}
```

Finally to wrap all of this up will need to create a function that will add append each box we create to the Dom like so

```javascript
function update() {
  for (let i = 0; i < boxes.length; i++) {
    boxStorage.appendChild(boxes[i]);
  }
}
// also update needs to be called when ever we create a new box
button.addEventListener('click', () => {
  addBox(boxName.value, boxColor.value, boxHeight.value, boxWidth.value);
  update(); // add this to event listener
  boxName.value = '';
});
```

keep in mind that the `initBox` method pushes the `div` element into the boxes array and we are importing boxes array from `clasess.js` 

Now when all is said and done we should be able to create a box and give it a name, color, height and width.

Final Result 

![image-20211117180952625](/home/camer/.config/Typora/typora-user-images/image-20211117180952625.png)

## Conclusion

Hopefully you found this tutorial useful in some way or another. It is very useful especially if you've been asked to create a VanillaJS project for a future employer to prove you skills to be able to modularize code with out using web pack or node. If you have any trouble with this tutorial or have any suggested edits I encourage you to leave a comment or if there is any thing you'd like me to explain in further detail.

