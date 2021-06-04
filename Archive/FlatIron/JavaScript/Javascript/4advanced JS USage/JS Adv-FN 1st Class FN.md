# lab

```js
const returnFirstTwoDrivers = function(array) {
	const newArray = [];
	newArray.push(array[0]);
	newArray.push(array[1]);
	return newArray;
};

const returnLastTwoDrivers = function(array) {
	const newArray = [];
	newArray.push(array[array.length - 2], array[array.length - 1]);
	return newArray;
};

const selectingDrivers = [ returnFirstTwoDrivers, returnLastTwoDrivers ];

const createFareMultiplier = (num) => {
	return function fareMultiplier(fare) {
		return fare * num;
	};
};

const fareDoubler = (lone) => {
	return createFareMultiplier(2)(lone);
};

const fareTripler = (fare) => {
	return createFareMultiplier(3)(fare);
};

const selectDifferentDrivers = (array, func) => {
	return func(array);
};

```

