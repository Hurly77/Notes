**JSON** is a **language-agnostic** way of **formatting** **data**. If we send a web request to the Game of Thrones **API**, it will **return** to us a **JSON collection** of **data**. With just one easy line of code, we can tell JavaScript to treat that JSON collection as a nested `Object`. In this way, large and complicated amounts of data can be shared across platforms.

Getting data from the [Game of Thrones](https://anapioficeandfire.com/) API with `fetch()`

```jsx
fetch('https://anapioficeandfire.com/api/books')
  .then(resp => resp.json())
  .then(json => console.log(json));
```

<img src="C:\Users\camer\AppData\Roaming\Typora\typora-user-images\image-20200819164056560.png" alt="image-20200819164056560"  />

We can use the `json()` method of the `Body` mixin to render the API's response as JSON. We then pass the arrow function's result to the *next* `then()`. Thus in the second `then()` we receive a JSON `String` that, when we pass it to `console.log()` prints a JavaScript object to our console.

![image-20200819164404960](C:\Users\camer\AppData\Roaming\Typora\typora-user-images\image-20200819164404960.png)

The response from the API contains all ten books currently existing in the Game of Thrones series, in a JSON format.

# lab solution

```js
function fetchBooks() {
  return fetch("https://anapioficeandfire.com/api/books")
  .then(resp => resp.json())
  .then(json => renderBooks(json))
}

function renderBooks(books) {
  const main = document.querySelector('main')
  books.forEach(book => {
    const h2 = document.createElement('h2')
    h2.innerHTML = book.name
    main.appendChild(h2)
  })
}

document.addEventListener('DOMContentLoaded', function() {
  fetchBooks()
});
```

