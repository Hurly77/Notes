## The Problem

imagine we have some `animateDiv.js` script we want browsers to receive that itself makes use of `jquery`. The first file we send to a requesting client, `index.html`, may look like this:

```jsx
<!-- index.html -->
<html>
  <head>
    <meta charset="utf-8">
    <script src="jquery.js"></script>
    <script src="animateDiv.js"></script>
    <title>Discotek</title>
  </head>
  <body>
    <div class="animat" onclick="animateDiv.js">
      I'm going to animate if you click me!
    </div>
  </body>
</html>
```

With this approach, we are actually making three http requests to the server for the application:

- We hit the base url and are returned the `index.html` file
- `index.html` tells the browser to request `jquery.js` from the server
- `index.html` tells the browser to request `animateDiv.js` from the server

A quick and dirty way around this would be to combine our JavaScript files into one file on the server (bringing this to two requests):

```jsx
<!-- index.html -->
...
<script src="combinedJqueryAnimateDiv.js"></script>
...
```

though this isn’t practical

**Webpack**  - lets us combine different files automatically. this means that we can freely import external js code in out Js files  (both local files as well as `node_modules` installed with `npm`). 

We trust that Webpack, before we send clients our JS code over the internet, intelligently packages it up for us. In a **simplified example:**

- File `siliconOverlord.js` has space-age AI code in it
- File `enslaveHumanity.js` wants to make use of this other file and send it to browsers all over the internet.

**Webpack** pre-bundles them together into a single file that can be sent instead: `singularity.js`

when you compile react app with **Webpack**, It’ll check every file for dependencies that it needs to import. This will give us one big JS file. that means we only need to send on file to the client.

### Simplified Webpack example

The files we want our client to have, which constitute one whole dank web application:

```jsx
// reveal.js (pre Webpack digestion)
function reveal(person, realIdentity) {
  person.identity = realIdentity
}
 
export default reveal
```

```jsx
// main.js (pre Webpack digestion)
import reveal from './reveal.js'
 
const gutMensch = {
  name: "Andrew Cohn",
  identity: "Friendly Neighborhood Flatiron Teacher",
}
 
reveal(gutMensch, "Chrome Boi")
```

**without Webpack**, we would need to find some way to send both files to our client and ensure they are playing nicely together

**The result after we unleash Webpack on these files:**

```jsx
// bundle.js (post Webpack digestion)
function reveal(person, realIdentity) {
  person.identity = realIdentity
}
 
const gutMensch = {
  name: "Andrew Cohn",
  identity: "Friendly Neighborhood Flatiron Teacher",
}
 
reveal(gutMensch, "Chrome Boi")
```

