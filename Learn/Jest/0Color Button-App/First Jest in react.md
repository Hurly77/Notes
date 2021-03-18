TDD, what is TDD and why is it important? TDD is as your probably already know **T**est **D**riven **D**evelopment. TDD Is Very important for many reasons. First, it forces attention to detail, that way we don’t double back over all our code just to find out we have a very simple syntax error. Secondly, testing our application helps us conceptualize what is going on behind the the smoke and mirrors.

Now let me express a personal opinion when it comes to TDD. Now like I said this is my opinion and I always keep an open mind to hearing other perspectives but, I believe the best TDD pattern is to test first and code second. I believe test first development is more productive for the following reasons

- Better planning,
- no double backing to write tests
- help stop problems before they start etc.

There are many reason I believe that test first development the best approach however you didn’t come here to listen to my opinions. So lets get start writting our first test.

If your using `npx create-react-app` jest and react testing library come out of the box. However if your building a react app from scratch you’ll have to set up jest `jest` `react` scripts by using web-pack and babel. 

After you set up react  you’ll should find a file in the `src/app.test.js` in the file you’ll find something that looks like this

```jsx
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => {
  render(<App />);
  const linkElement = screen.getByText(/learn react/i);
  expect(linkElement).toBeInTheDocument();
});
```

So lets break this down line by line

```js
//first we import render @testing-library.
import {render, screen} from '@testing-library/react';
import App from './App';
```

If you go to https://testing-library.com/docs/queries/about/#screen a complete description. But accentually it allows us to query the virtual DOM the way you would do in the dev console with some subtle difference.

```jsx
// test is a global meth that comes with jest
test('renders learn react link', () => {
    //we render the <App /> react component wich should contain any and every thing that we would need 
	render(<App />);
// )
	const linkElement = screen.getByText(/learn react/i);//  /learn react/i is regular expression and is pretty complext so i would suggest doing some reseach on that if you un fimiliar with it
	
    //expect is a function that we get from jest, this function ussually used in tandem with .toBe() and expect accepts someValue as its argument and .toBe accepts an expected usaully scalar value such as a string or an integer.
    expect(linkElement).toBeInTheDocument();
})
```

now simply run `npm test` and you should enter `watch mode` what mode will allow you to choose how you want to run test whether you do them all at once or just the one that aren’t passing or all the one that are “I believe” don’t quote me on that I’m still a new to jest my self.

Okay you have officially ran you tests and should see a screen that shows all test are passing.

Now that we that we got that out of the way lets move on to something slightly more advanced using what we learn from the above code context.

```jsx
test('button has correct initial color', () => {
  render(<App />);
  // find an element with a role of button and text of 'Change to blue'
  const colorButton = screen.getByRole('button', {name: 'Change to blue'})
  expect(colorButton).toHaveStyle({backgroundColor: 'red'})
});
```

Here we do the same thing as we did before only this time we use `getByRole` instead of `getByText` and `toHaveStyle` instead of `toBeInTheDocument`. If you go back to https://www.w3.org/TR/wai-aria/#role_definitions you’ll find a list of roles and definitions. The first argument in `getByRole` define what the role should be and the second  argument specifies what the button should be called hence `name`.

To pass the test will need to add this to the App.js file.

```jsx
return (
    <div>
      <button style={{backgroundColor: 'red'}}>
        Change to blue
      </button>
    </div>
  );
```

and boom you’ve just written your first official test.