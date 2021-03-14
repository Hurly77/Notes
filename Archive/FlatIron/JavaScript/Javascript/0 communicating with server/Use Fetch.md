browsers won't show anything until they've processed all the of that data.



## VOCAB

> - ***Serialization format***  -  In computing, **serialization** is the process of translating a data structure or **object** state **into** a **format** that can be **stored** or **transmitted** and **reconstructed** later. The **resulting** series of **bits** is reread according to the **serialization format.**
> - ***asynchronous Input / Output*** -  asynchronous programing lets other task chug along on another processor.  
> -  ***The event loop*** -  is responsible for **executing** the code, **collecting** and **processing** events, and **executing** queued **sub-tasks**

------------------------------------------------------------------------------------------------------------------------------------------

To solve this problem and help provide lots of other really great features, we developed a technique called **AJAX**.

In AJAX we:

1. Deliver an initial, engaging page using HTML and CSS which browsers render *quickly*
2. *Then* we use JavaScript to add more to the DOM, behind the scenes

AJAX relies on several technologies:

- Things called `Promise`s
- Things called `XMLHttpRequestObject`s
- A [serialization format](https://en.wikipedia.org/wiki/Serialization) called JSON for "JavaScript Object Notation"
- [asynchronous Input / Output](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Introducing)
- [the event loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop)

To **understand**  **AJAX**  *thoroughly*, you need to understand ***all* these components**. Luckily for now **browsers** have *abstracted* all those components into a single function called `fetch()`.

## How to Fetch Data with `fetch()`

The `fetch()` function retrieves data. **It's a global *method*** on the `window` object. That means you can simply use it with `fetch()` in DevTools.

 quick reference, here's the skeleton:

```js
fetch("string representing a URL to a data source")
.then(function(response) {
  return response.json();
})
.then(function(json){
  // Use this data inside of `json` to do DOM manipulation
})
```

in-line JavaScript comments to help us track what's going on

```js
fetch("string representing a URL to a data source")
  /*
    The function above returns an object that represents what the data source
    sent back. It does *not* return the actual content.
    We have to call the then() method on the object that comes back. then()
    takes as its argument a function that receives the response as its argument.
    Inside of the function, we do whatever processing we need, but at the end we
    *have to return* the content that we've gotten out of the response.
    The response has some basic functions on it for the most common data types.
    The most important ones are .json() and .text().
    This callback function is usually only one line: returning the content from
    the response. What we return from this function will be used in the _next_
    then() function.
  */
 
  .then(function(response) {
    return response.json();
  })
 
  /*
    The function above returns the content from the response.
    We can use that content inside of the callback function that's
    passed in to the then() function below.
  */
 
  .then(function(json){
    // Use this data inside of `json` to do DOM manipulation
  })
```

#### fill out our base skeleton.

When you first start using `fetch()` most of your first `then()`s are going to look like this:

```js
function(response) {
  return response.json();
}
```

something with the JSON. The easiest options are

- `alert()` the JSON
- `console.log()` the JSON
- hand the JSON off to another function.

We'll go for the `console.log()` approach:

```js
function(json) {
  console.log(json)
}
```

##### working code:

```js
fetch('http://api.open-notify.org/astros.json')
.then(function(response) {
  return response.json();
})
.then(function(json) {
  console.log(json)
});
```

###### Working Around Backwards Compatibility Issues

However, `fetch()` has only recently arrived in browsers. In older code you might see `jquery.ajax` or `$.ajax` or an object called an `XMLHttpRequestObject`. These are distractions at this point in your education. After working with `fetch()` you'll be able to more easily integrate these special topics.

## Identify Examples of the AJAX Technique on Popular Websites

The AJAX technique opens up a lot of uses!

- It allows us to pull in dynamic content. The same "framing" HTML page remains on screen for a cooking website. The recipe on display updates *without* page load. This approach was pioneered by GMail whose nav area is swapped for mail content swiftly — thanks to AJAX.
- It allows us to get data from multiple sources. We could make a website that displays the current weather forecast and the current price of bitcoin side by side! This approach is used by most sites to render ads. Your content loads while JavaScript gets the ad to show and injects it into your page (sometimes AJAX can be used in a way that we don't *entirely* like).

Using `fetch()`, we can include requests for data wherever we need to in our code. We can `fetch()` data on the click of a button or the expansion of an accordion display. There are many older methods for fetching data, but `fetch()` is the future.

## Resources

# reading questions

1. Explain how to fetch data with `fetch()`

   > Pass in a string like so fetch('string representing data such as an API')
   >
   > call then on the object that comes back as argument 
   >
   > ```js
   > .then(function(objectOrResponse){
   >   	 //**whatever processing we need**, *but* at the end we;
   >    
   >      //**have to *return*** the content that we've gotten out of the 		response;	
   >    });
   > //what is return in this function is what we use in the next then function
   >  .then(function(objectOrResponse) {
   >     return objectOrResponse.json();
   >      //returns the content from the response.
   > 	 //passed in to the then() function below.
   >   })
   > .then(function(json){
   >     // Use this data inside of `json` to do DOM maniplation
   >   })
   > ```


