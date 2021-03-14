**Definitions**

1. **Execution** Context: When JavaScript functions run, they have an associated JavaScript `Object` that goes along with them which they can access by the keyword `this`.
2. **this** : Inside a function, `this` is the `Object` that represents the function's execution context
3. **call**: This is a method *on a function* that calls the function, just like `()`. You provide a new execution context as the first argument, traditionally called `thisArg`, and the arguments you want to send to the function after the `thisArg`. An invocation of `call` looks like: `Calculator.sum.call(multilingualMessages, 1, 2)`
4. **apply**: This is a method *on a function* that calls the function, just like `()`. You provide a new execution context as the first argument, traditionally called `thisArg`, and the arguments you want to send to the function ***as an `Array`\*** after the `thisArg`. An invocation of `apply` looks like: `Calculator.sum.apply(multilingualMessages, [1, 2])`
5. **bind**: This method returns *a copy* of the function but with the execution context "set" to the argument that's passed to `bind`. It looks like this: `sayHello.bind(greenFrog)("Hello") //=> "Mr. GreenFrog says *Hello* to you all."`

## Define the Term "Record-Oriented Programming"

[Record-oriented programming](https://en.wikipedia.org/wiki/Record_(computer_science)) **-** is a style of programming based on **finding records and processing them** so that they're updated (`map`-like) or so that their information is aggregated (`reduce`-like).

# lab

```js
function createEmployeeRecord(record) {
	let emp = {
		firstName: record[0],
		familyName: record[1],
		title: record[2],
		payPerHour: record[3],
		timeInEvents: [],
		timeOutEvents: []
	};
	return emp;
}

function createEmployeeRecords(records) {
	return records.map((e) => {
		return createEmployeeRecord(e);
	});
}

function createTimeInEvent(record, timeIn) {
	let [ date, hour ] = timeIn.split(' ');
	record.timeInEvents.push({ date: date, hour: parseInt(hour), type: 'TimeIn' });
	return record;
}

function createTimeOutEvent(record, timeOut) {
	let [ date, hour ] = timeOut.split(' ');
	record.timeOutEvents.push({ date: date, hour: parseInt(hour), type: 'TimeOut' });
	return record;
}

function hoursWorkedOnDate(record, date) {
	let timeIn = record.timeInEvents.find((obj) => {obj.date == date;});
	let timeOut = record.timeOutEvents.find((obj) => {obj.date == date;});
	return (timeOut.hour - timeIn.hour) / 100;
}

function wagesEarnedOnDate(record, date) {
	let hrs = hoursWorkedOnDate(record, date);
	return hrs * record.payPerHour;
}

function allWagesFor(record) {
	let wages = record.timeInEvents.map((obj) => {
		return wagesEarnedOnDate(record, obj.date);
	});
	return wages.reduce(function(a, b) {
		return a + b;
	});
}

function findEmployeeByFirstName(src, firstName) {
	return src.find(function(emp) {
		return emp.firstName === firstName;
	});
}

function calculatePayroll(records) {
	let pay = records.map(function(obj) {
		return allWagesFor(obj);
	});
	return pay.reduce(function(a, b) {
		return a + b;
	});
}
```

