# Getting Started With CSS

We need to set up a grid, will do this with css `grid` and add 21 `cols` and `row`

```css
#game-board {
    height: 100vmin;
    width: 100vmin; /*sets the hight to and width to the smallest viewport*/
	display: grid;
    grid-template-rows: repeat(21, 1fr);
    grid-template-columns: repeat(21, 1fr)
}
```

First will need to create a `game.js` file that will contain code for our snake game. Also will wan to use `import` so we have to make sure the script loads as `type='module'`. To do that all we need is put this in th e`head` of our HTML document.

```html
<script src="game.js" defer type="module"></script>
<!-- The " defer " attribute will make sure that the script loads after the Dom content -->
```

# Getting started with JavaScript

Our first function will go in our game.js file, will want to incorperate a `main` function that will act as our **`game loop`** so to speak. This will update the postion of the snake and any code that runs along with it as to have a more fluent game.

```javascript
let lastRenderTime = 0
const SNAKE_SPEED = 2 //times per second

export function main(currentTime){ // this is the time stamp
    const secondsSinceLastRender = (currentTime = lastRenderTime) /1000
    secondsSince < 1 /SNAKE_SPEEND
	window.requestAnimationFrame(main)
}
// this is a recursive function which means that it will call its self

  window.requestAnimationFrame(main) // will start the game
 
```

let add a console.log to our main function and 

```javascript
function main(currentTime) {
...
  console.log('render')
    lastrenderTime = currentTime
  }
```

will add Draw function and a update function, and add a file `snake.js`

in `snake.js` file will add 

```javascript
export const SNAKE_SPEED = 5 // how fast it moves per second
export function update() {
    console.log('update')
}

export function draw(gameBoard) {
    console.log('draw')
}
```



```javascript
import { update as updateSnake, draw as drawSnake, SNAKE_SPEED, getSnakeHead, snakeIntersection } from './snake.js'
function update() {
 
}

function draw() {
}
```

