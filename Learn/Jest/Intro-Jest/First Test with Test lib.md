```jsx

import { render, screen } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => {
    //the render method creates a virtual dom for whatever jsx you give it as and argument.
  render(<App />);
     //we can manipulte the jsx using the global screen object.
  const linkElement = screen.getByText(/learn react/i);
  	//we set the linkElement to be the screen object get by text 
  expect(linkElement).toBeInTheDocument();
});
```

```js
//expect - Jest global, starts the assertion
expect(linkElement).toBeInDocument();
//expect argument- subject of the assertion.
//toBeInDocument - does not accept and argument.
//Examples
expect(element.textContent).toBE('hello');
expect(elementsArray).toHaveLength(7);
```

**React Testing Library**

> - jest-dom
>   - comes with create-react-app
>   - src/setupTests.js imports it before each test, makes matchers available
>   - Dom-based matchers
>     - examples: `toBeVisible()` or `toBeChecked()`
>
> - React Testing Library helps with
>   - rendering components into virtual DOM
>   - searching virtual DOM
>   - Interacting with virtual OM
> - Needs a test runner
>   - Find tests, run them, make assertions
> - Jest
>   - is recommended by Testing Library
>   - comes with create-react-app
> - `npm test` runs an npm script that runs jest in watch mod

**Jest Watch Mode**

> - Watch for changes in files since las commit
> - Only run tests related to these files
> - No changes? No tests
>   - Type `a` to run all tests

**How Jest Works**

> - global `test` method has two arguments:
>   - string description
>   - test function
> - Test fails if error is thrown when running function 
>   - \assertions throw errors when expectation fails
> - No error => tests pass
>   - Empty test should past