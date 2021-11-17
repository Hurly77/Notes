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

Note: **If you here to just get the answer to "how" copy and paste the below**

```html
<script src="./js/main.js" defer type='module'></script>
```

