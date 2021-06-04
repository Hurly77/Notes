### Synchronous vs. Asynchronous

**Definition** 

**Synchronous**  - existing or occurring at the same time.

**Asynchronous** - (of **two or more** objects or events) not existing or happening at the same time

Accentually there are a  both **synchronous** and **asynchronous**  process in JavaScript, JS delegates between what is which.

## How to recognize **Async** Vs. **Sync** 

As we have experienced in **JavaScript**, our code **executes** from **top-to-bottom**  and ***left-to-right.***

```js
function getData(){
  console.log("2. Returning instantly available data.")// Hits SECOND
  return [{name: "Dobby the House-Elf"}, {name: "Nagini"}] //array with obj inside
}
 
function main(){
  console.log("1. Starting Script") //Hits FIRST
  const data = getData()
  console.log(`3. Data is currently ${JSON.stringify(data)}`)//Hits Third
  console.log("4. Script Ended")//Hits Fourth
}
 
main()
```

## Identify an Asynchronous Code Bloc

The easiest **Async** wrapper function is `window.setTimeout()`It takes as arguments

- a `Functio`(the "callback" function)
- a `Number` representing milliseconds 

```js
setTimeout(() => console.log('Hello World!'), 2000)
```

# **Sending Data with Fetch Lab**

##  JSON Server

```
npm install -g json-server`. To start up JSON Server, run `json-server --watch db.json
```

## HTML Form

```html
<form action="http://localhost:3000/dogs" method="POST">
  <label> Dog Name: <input type="text" name="dogName" id="dogName" /></label><br />
  <label> Dog Breed: <input type="text" name="dogBreed" id="dogBreed" /></label><br />
  <input type="submit" name="submit" id="submit" value="Submit" />
</form>
```

#### Construct a POST Request Using `fetch()`

`fetch()` can also take a JavaScript `Object` (`{}`) as the *second* argument. This `Object` can be given certain [properties](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch#Parameters) with certain values in order to change `fetch()`'s default behavior.

```js
fetch(destinationURL, configurationObject);
```

### Add the HTTP Verb

we need is to include the **HTTP** verb. By **default**, the verb is **GET**, which is why we can **send** simple **GET** requests **with *only*** a **destination URL**. To tell `fetch()` that this is a POST request, we need to add a `method` key to our `configurationObject`:

```js
configurationObject = {
  method: "POST"
};
```

### Add Headers

we also need to **include** is some ***metadata*** about the actual data we want to send. This metadata is in the form of [*headers*](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers). **Headers** - are **sent** just **ahead** of the actual **data** payload of our POST request. They contain information about the data being sent.

A common header is  `"Content-Type"` is used to indicate what format the data being sent is in. `fetch()`, [JSON](https://www.json.org/) is the most common format we will be using. Make sure that the destination of your POST request knows to include the `"Content-Type"` header:

```JS
configurationObject = {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  }
};
```

**header** is stored as a **key/value** pair inside an object This object is assigned to the `headers` key as seen above. the server at the destination URL will send back a response often including data that the sender of the `fetch()` request might find useful.  Just like `"Content-Type"` tells the destination server what type of data we're sending, it is also good practice to tell the server what data format we *accept* in return.

To do this, we add a second header, [`"Accept"`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept), and assign it to `"application/json"` as well:

```JS
configurationObject = {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "Accept": "application/json"
  }
};
```

There are many headers `"Content-Type"` and `"Accept"` are two that we'll see the most throughout the remainder of this course.

Servers may reject requests without the specific headers the server is configured to expect.

### Add Data

Data being sent in `fetch()` must be stored in the `body` of the `configurationObject`:

```JS
configurationObject = {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "Accept": "application/json"
  },
  body: /* Your data goes here */
};
```

There is a catch here to be aware of - when data is exchanged between a client (your browser, for instance), and a server, the data is sent as *text*. Whatever data we're assigning to the `body` of our request needs to be a string.

#### Use `JSON.stringify()` to Convert Objects to Strings

we often organize this information using objects.  for instance:

```js
{
  dogName: "Byron",
  dogBreed: "Poodle"
}
```

We can't simply assign it to `body`, as it isn't a string. Instead, we convert it to JSON.

```json
"{"dogName":"Byron","dogBreed":"Poodle"}"
```

When sent to a server, the server will be able to take this string and convert it back into key/value pairs in whate for converting objects to strings, `JSON.stringify()`ver language the server is written in.

## Send the POST Request

Putting it all together, we get:

```Js
fetch("http://localhost:3000/dogs", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "Accept": "application/json"
  },
  body: JSON.stringify({
    dogName: "Byron",
    dogBreed: "Poodle"
  })
});
```

we don't have to define everything inside of one anonymous `Object`. We could also write (they're exactly the same!):

```js
let formData = {
  dogName: "Byron",
  dogBreed: "Poodle"
};
 
let configObj = {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "Accept": "application/json"
  },
  body: JSON.stringify(formData)
};
 
fetch("http://localhost:3000/dogs", configObj);
```

**Note**: As a security precaution, most modern websites block the ability to use `fetch()` in console while on their website, so if you are testing out code in browser, make sure to be on a page like `index.html` or `sample_form.html`.

## Handling What Happens After

servers will send a [Response][response] To access this information, we use a series of calls to `then()` which are given function *callbacks*.

Building on the previous implementation

```js
let formData = {
  dogName: "Byron",
  dogBreed: "Poodle"
};
 
let configObj = {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "Accept": "application/json"
  },
  body: JSON.stringify(formData)
};
 
fetch("http://localhost:3000/dogs", configObj)
  .then(function(response) {
    return response.json();
  })
  .then(function(object) {
    console.log(object);
  });
```

first `then()` is passed a callback function that takes in `response` as an argument. This is a [`Response`][response] object, representing what the destination server sent back to us This object has a built in method, `json()`, that converts the *body* of the response from JSON to a plain old JavaScript object.

The result of `json()` is returned and made available in the *second* `then()`. In this example, whatever `response.json()` returns will be logged in `console.log(object)`.

Sending the example above to our JSON server, once the request is successfully resolved, we would see the following log:

{**dogName**: "Byron", dogBreed: "Poodle", id: 6} *// Your ID will vary depending*

### When Things Go Wrong: Using `catch()`

When something goes wrong in a `fetch()` request, JavaScript will look down the chain of `.then()` calls for something very similar to a `then()` called a `catch()`.

When something goes wrong in a `fetch()` request, JavaScript will look down the chain of `.then()` calls for something very similar to a `then()` called a `catch()`.

```js
let formData = {
  dogName: "Byron",
  dogBreed: "Poodle"
};
 
// method: "POST" is missing from the object below
let configObj = {
  headers: {
    "Content-Type": "application/json",
    "Accept": "application/json"
  },
  body: JSON.stringify(formData)
};
 
fetch("http://localhost:3000/dogs", configObj)
  .then(function(response) {
    return response.json();
  })
  .then(function(object) {
    console.log(object);
  })
  .catch(function(error) {
    alert("Bad things! Ragnarők!");
    console.log(error.message);
  });
```

Sent to our JSON server from a page like `index.html`, we would receive an alert window pop-up and a logged message:

```
Failed to execute 'fetch' on 'Window': Request with GET/HEAD method cannot have body.
```

While `catch()` may not stop *all* silent errors, it is useful to have as a way to gracefully handle unexpected results. We can use it, for instance, to display a message in the DOM for a user, rather than leave them with nothing.

# lab solution

### Test 2 - Handle the Response

On a successful POST request, expect the server to respond with a [`Response`][response] object. Just like we saw earlier in the dog example, the `body` property of this response will contain the data from the POST request along with a newly assigned *id*.

Use a `then()` call to access the `Response` object and use its built in `json()` method to parse the contents of the `body` property. Use a *second* `then()` to access this newly converted object. From this object, find the new id and append this value to the DOM.

Using `index.html` and the JSON server, if your code is successful, calling `submitData` in the console should cause an id number to appear on the page.

### Test 3 - Handle Errors

For this final test, after the two `then()` calls on your `fetch()` request, add a `catch()`.

When writing the callback function for your `catch()`, expect to receive an object on error with a property, `message`, containing info about what went wrong. Append this message to the DOM when `catch()` is called.

### Test 4 - Return the Fetch Chain

An amazing feature of `fetch()` is that if you *return* it, *other* functions can tack-on *their own* `then()` and `catch()` calls. While we won't explore this amazing idea in this lesson, let's learn good habits and be sure to return the `fetch()` chain from our `submitData` function.

```js
function submitData(name, email) {
  let nameAndEmail = [];
  let div = document.getElementById('stuff');
	return fetch('http://localhost:3000/users', {
    method: 'post',
		headers: {
      'Content-Type': 'application/json',
			Accept: 'application/json'
		},
		body: JSON.stringify({
      name: `${name}`,
			email: `${email}`
		})
	})
  .then(function(response) {
    return response.json();
  })
  .then(function(object) {
    document.body.innerHTML = object.id
  })
  .catch(function(error) {
    document.body.innerHTML = error.message;
  });
}
```

