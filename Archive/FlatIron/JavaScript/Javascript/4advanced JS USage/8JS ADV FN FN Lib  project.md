# functional programming: Lab

## functional programing Vocab

- **Pure functions -** 

  > Is a function which
  >
  > - Given the same inputs, always returns the same output, and
  > - Has no side-effects
  >   - **Pure functions** have lots of properties that are important in functional programming, including **referential transparency** (you can replace a function call with its resulting value without changing the meaning of the program). Read [“What is a Pure Function?”](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976) for more details.

- **Function composition**

  > **Function composition** is the process of combining two or more functions in order to produce a new function or perform some computation. For example, the composition `f . g` (the dot means “composed with”) is equivalent to `f(g(x))` in JavaScript. Understanding function composition is an important step towards understanding how software is constructed using the functional programming. Read [“What is Function Composition?”](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-function-composition-20dfb109a1a0) for more.

- **Avoid shared state**

  > **Shared state** is any variable, object, or memory space that exists in a shared scope, or as the property of an object being passed between scopes. A shared scope can include global scope or closure scopes. Often, in object oriented programming, objects are shared between scopes by adding properties to other objects.
  >
  > For more details on how functional software might handle application state, see [“10 Tips for Better Redux Architecture”](https://medium.com/javascript-scene/10-tips-for-better-redux-architecture-69250425af44).

- **Avoid mutating state**

- **Avoid side effects**

```js
const fi = (function() {
	return {
		libraryMethod: function() {
			return 'Start by reading https://medium.com/javascript-scene/master-the-javascript-interview-what-is-functional-programming-7f218c68b3a0';
		},

		each: function(set, recall) {
			if (Array.isArray(set)) {
				for (const e of set) {
					recall(e);
				}
				return set;
			}

			if (typeof set === 'object') {
				for (const key in set) {
					recall(set[key]);
				}
				return set;
			}
		},

		map: function(set, task) {
			let maped = [];
			for (const key in set) {
				maped.push(task(set[key]));
			}
			return maped;
		},

		reduce: function(set, recall, qua) {
			if (!qua) {
				qua = set[0];
				set = set.slice(1);
			}
			for (let i = 0; i < set.length; i++) {
				qua = recall(qua, set[i], set);
			}
			return qua;
		},

		find: function(set, predicate) {
      for(const e of set){
        if(predicate(e)){
          return e
        }
      }
    },

		filter: function(set, predicate) {
      let tru = []

      for(const e of set){
        if(predicate(e)){
          tru.push(e)
        }
      }
      return tru
    },

		size: function(set) {
        return Object.keys(set).length
    },

		first: function(set, x) {
      let a = []
      if(!x){return set[0]}
      for(const e of set){
        if(e == x){
          a.push(e)
          return a
        }else{
          a.push(e)
        }
      }
      return a
    },

		last: function(set, n) {
      if(!n){return set[set.length - 1]}
      if(n){
        return set.slice(set.length - n)
      }
    },

		compact: function(set) {
      let newArr = [];
			for (const i in set) {
				if (!!set[i]) {
					newArr.push(set[i]);
				}
			}
			return newArr;
    },

		sortBy: function(array, callback) {
			return [ ...array ].sort((a, b) => callback(a) - callback(b));
		},

		flatten: function(array, shallow = false) {
			if (shallow) {
				return array.flat();
			} else {
				return array.flat(Infinity);
			}
		},

		uniq: function(array, isSorted, callback) {
			if (!callback || isSorted) {
				return [ ...new Set(array) ];
			} else {
				let callbackVals = new Set();
				let uniqVals = new Set();
				for (let val of array) {
					let callbackVal = callback(val);
					if (!callbackVals.has(callbackVal)) {
						callbackVals.add(callbackVal);
						uniqVals.add(val);
					}
				}
				return Array.from(uniqVals);
			}
		},

		keys: function(obj) {
			return Object.keys(obj);
		},

		values: function(obj) {
			return Object.values(obj);
		},

		functions: function(obj) {
			let arr = [];
			for (let key in obj) {
				if (typeof obj[key] === 'function') {
					arr.push(key);
				}
			}
			return arr.sort();
		}
	};
})();

fi.libraryMethod();
```

