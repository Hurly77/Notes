####  *relative* path

Relative paths start with `./` which means "from the directory I am currently in." So, when we use `link` to associate with a stylesheet and we write `href="./style.css"` we're saying: "From the directory in which I, the `index.html` file live, look for a file called `style.css` and use it.

```css
body {
  background-color: #ffece6;
  width: 100%;
  height: 100%;
  margin: 0;
}

header {
  text-align: center;
  align-content: center;
  align-self: center;
}

section#featured-property img{
  display: block;
 margin-left: auto;
 margin-right: auto;
}
section#featured-property p {
  text-align: center;
  margin-left: auto;
  margin-right: auto;
  width: 800px;
}

nav > a {
  padding-left: 10px;
  padding-right: 10px;
  background-color: #ccfdff;
}

section figcaption {
  text-align: center;
  font-size: 80%;
}


section#details div{
  background-color: #4df9ff;
  display: block;
  float: left;
  width: 25%;
  text-align: center;
}
```

