## Practice Moving Elements on the Page

```css
#dodger {
  background-color: white;
  height: 20px;
  position: absolute;
  width: 40px;
}

#game {
  background-color: #111;
  height: 400px;
  margin: 0 auto;
  overflow: hidden;
  position: relative;
  text-align: center;
  width: 400px;
}

#start {
  color: white;
  font-size: 4rem;
  position: relative;
  text-decoration: none;
  top: 4rem;
}

.rock {
  width: 20px;
  height: 20px;
  background: tan;
  position: absolute;
}

.rock:before {
  content: "";
  border-bottom: 6px solid tan;
  border-left: 6px solid #111;
  border-right: 6px solid #111;
  height: 0;
  left: 0;
  position: absolute;
  top: 0;
  width: 9px;
}

.rock:after {
  content: "";
  border-left: 6px solid #111;
  border-right: 6px solid #111;
  border-top: 6px solid tan;
  bottom: 0;
  height: 0;
  left: 0;
  position: absolute;
  width: 9px;
}

```

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Rock Dodger! (Beta)</title>
  <link rel="stylesheet" type="text/css" href="index.css">
</head>
<body>
  <div id="game">
    <div id="dodger" style="bottom: 0px; left: 180px;"></div>
  </div>
  <script type="text/javascript" src="index.js"></script>
</body>
</html>
```

## code along

grab Element we want to move

```js
let dodger = document.getElementById("dodger");
```

 change its color:

```js
dodger.style.backgroundColor = "#FF69B4";
```

grab coordinates, the bottom left of the box = (0, 0)

```js
console.log(dodger.style.left); // "180px"
console.log(dodger.style.bottom); // "0px"
```

 moving the element up.

```js
dodger.style.bottom = "100px";
```

##### Demonstrate How to Move an element in response to a browser event

figure what the left arrow key's numeric value is:

```js
document.addEventListener("keydown", function(e){
console.log(e.key);
})
```

This will log

# lab

```js
let dodger = document.getElementById("dodger");

dodger.style.backgroundColor = "#FF69B4";

// dodger left
  function moveDodgerLeft() {
      let leftNumbers = dodger.style.left.replace("px", "");
      let left = parseInt(leftNumbers, 10);

      if(left > 0){ 
      dodger.style.left = `${left - 1}px`;
    }
  }

  document.addEventListener("keydown", function(e) {
    if (e.key === "ArrowLeft") {
      moveDodgerLeft();
    }
    if (e.key === "ArrowRight") {
      moveDodgerRight();
    }
  });

  //dodger right
  function moveDodgerRight(){
      let leftNumbers = dodger.style.left.replace("px", "");
      let left = parseInt(leftNumbers, 10);

      if(left < 360){ 
        dodger.style.left = `${left + 1}px`;
      console.log(left);
      }
  }
```

