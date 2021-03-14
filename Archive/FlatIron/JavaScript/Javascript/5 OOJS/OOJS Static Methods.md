### Calling a `Static` Method

As the `static` method is operating on the class, you call the `static` method directly on the class.

```JS
ClassName.methodName();
// Calls the method explicitly on the class name itself and returns the `static` value
```

For `static` methods, `this` references the class. This means that you can call a `static` method from within another `static` method of the same class using `this`.

# lab

```js
class Formatter {
	//add static methods here
	static capitalize(string) {
		return string.charAt(0).toUpperCase() + string.slice(1);
	}

	static sanitize(string) {
		return string.replace(/[^A-Za-z0-9-_' ]/g, '');
	}

	static titleize(string) {
	  let notCap = ['the', 'a', 'an', 'but', 'of', 'and', 'for', 'at', 'by', 'from']
    return string.split(' ')
      .map((w,i) => {
         if (i===0 || !notCap.includes(w)){
          return this.capitalize(w)
        }else {
          return w
        }
       })
      .join(' ')
    }
}
```

