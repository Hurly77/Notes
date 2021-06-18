![DOM Tree Graphic](https://curriculum-content.s3.amazonaws.com/fewpjs/fewpjs-the-dom-tree/Image_6_DomTree.png)

The HTML for this "tree" would be:

```html
<!DOCTYPE HTML>
<html>
  <head>
    <title>My Title</title>
  </head>
  <body>
    <h1>A heading</h1><a href="http://example.com">Link text</a>
  </body>
</html>
```

##### **`document.getElementById()`**

```html
<div>
  <h5 id="greeting">Hello!</h5>
</div>
```

We could find the `h5` element with `document.getElementById('greeting')`

*Note: You can use either single('') or double("") quotes around the `id` within the parentheses in `document.getElementById('yourIDGoesHere')`, as long as you use the same kind to open and close them!*

#### `document.getElementsByClassName()`



```html
<!-- the `className` attribute is called `class` in HTML  -->
<div>
  <div class="banner">
    <h1>Hello!</h1>
  </div>
 
  <div class="banner">
    <h1>Sup?</h1>
  </div>
 
  <div class="banner">
    <h5>Tinier heading</h5>
  </div>
</div>
```

We could find all of the elements with the class name "banner" by calling `document.getElementsByClassName('banner')`.

#### `document.getElementsByTagName()`

If you *don't* know an element's `id` or `class`, but you *do* know its tag name (the tag name is the thing between the `<>`, e.g., `'div'`, `'h1'`, `header`, `article` etc.). Since tag names aren't unique, this method returns an `HTMLCollection` also.

#### Finding a Node Without Knowing Anything About It

```javascript
const main = document.getElementsByTagName('main')[0]
const div = main.children[1]
// we can call getElementsByTagName() on an _element_
// to constrain the search to its children!
const p = div.getElementsByTagName('p')[0]
p.textContent = "Goodbye!"
```

