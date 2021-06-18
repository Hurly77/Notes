> ### `createEmployeeRecord`
>
> - Argument(s)
>   - A 4-element Array of a `String`, `String`, `String`, and `Number` corresponding to a first name, family name, title, and pay rate per hour
> - Returns
>   - JavaScript `Object` with keys:
>   - `firstName`
>   - `familyName`
>   - `title`
>   - `payPerHour`
>   - `timeInEvents`
>   - `timeOutEvents`
> - Behavior
>   - Loads `Array` elements into corresponding `Object` properties. *Additionally*, initialize empty `Array`s on the properties `timeInEvents` and `timeOutEvents`.
>
> ### `createEmployeeRecords`
>
> - Argument(s)
>   - `Array` of `Arrays`
> - Returns
>   - `Array` of `Object`s
> - Behavior
>   - Converts each nested `Array` into an employee record using `createEmployeeRecord` and accumulates it to a new `Array`
>
> ### `createTimeInEvent`
>
> - Argument(s)
>   - A date stamp (`"YYYY-MM-DD HHMM"`), where time is expressed in [24-hour standard](https://en.wikipedia.org/wiki/24-hour_clock)
> - Returns
>   - The record that was just updated
> - Behavior
>   - Add an `Object` with keys:
>   - `type`: Set to `"TimeIn"`
>   - `hour`: Derived from the argument
>   - `date`: Derived from the argument
>
> ### `createTimeOutEvent`
>
> - Argument(s)
>   - A date stamp (`"YYYY-MM-DD HHMM"`), where time is expressed in [24-hour standard](https://en.wikipedia.org/wiki/24-hour_clock)
> - Returns
>   - The record that was just updated
> - Behavior
>   - Add an `Object` with keys:
>   - `type`: Set to `"TimeOut"`
>   - `hour`: Derived from the argument
>   - `date`: Derived from the argument
>
> ### `hoursWorkedOnDate`
>
> - Argument(s)
>   - A date of the form `"YYYY-MM-DD"`
> - Returns
>   - Hours worked, an `Integer`
> - Behavior
>   - Given a date, find the number of hours elapsed between that date's timeInEvent and timeOutEvent
>
> ### `wagesEarnedOnDate`
>
> - Argument(s)
>   - A date of the form `"YYYY-MM-DD"`
> - Returns
>   - Pay owed
> - Behavior
>   - Using `hoursWorkedOnDate`, multiply the hours by the record's payRate to determine amount owed. Amount should be returned as a number.
>
> ### `allWagesFor`
>
> - Argument(s)
>   - *None*
> - Returns
>   - Sum of pay owed to all employees for all dates, as a number
> - Behavior
>   - Using `wagesEarnedOnDate`, accumulate the value of all dates worked by the employee in the record used as context. Amount should be returned as a number. **HINT**: You will need to find the available dates somehow....
>
> ### `findEmployeeByFirstName`
>
> - Argument(s)
>   - `srcArray`: Array of employee records
>   - `firstName`: String representing a first name held in an employee record
> - Returns
>   - Matching record or `undefined`
> - Behavior
>   - Test the `firstName` field for a match with the `firstName` argument
>
> ### `calculatePayroll`
>
> - Argument(s)
>   - `Array` of employee records
> - Returns
>   - Pay owed for all dates
> - Behavior
>   - Using `wagesEarnedOnDate`, accumulate the value of all dates worked by the employee in the record used as context. Amount should be returned as a number.

# lab

```js
/* Your Code Here */

let createEmployeeRecord = function(employee){
    let emp = {
        firstName: employee[0],
		familyName: employee[1],
		title: employee[2],
		payPerHour: employee[3],
		timeInEvents: [],
		timeOutEvents: []
	};
	return emp;
}

let createEmployeeRecords = function(employees){
    return employees.map((e) => {
        return createEmployeeRecord(e);
	});
}

let createTimeInEvent = function(timeIn){
    let [ date, hour ] = timeIn.split(' ');
    this.timeInEvents.push({ date: date, hour: parseInt(hour), type: 'TimeIn' });
	return this;
}

let createTimeOutEvent = function(timeOut){
    let [ date, hour ] = timeOut.split(' ');
    this.timeOutEvents.push({ date: date, hour: parseInt(hour), type: 'TimeOut' });
	return this;
}

let hoursWorkedOnDate = function(date){
    let timeIn = this.timeInEvents.find((obj) => {return obj.date == date;});
    let timeOut = this.timeOutEvents.find((obj) => {return obj.date == date;});
	return (timeOut.hour - timeIn.hour) / 100;
}

let wagesEarnedOnDate = function(date){
    let hrs = hoursWorkedOnDate.call(this, date);
	return hrs * this.payPerHour;
}

let allWagesFor = function () {
    let eligibleDates = this.timeInEvents.map(function (e) {
        return e.date
    })
    
    let payable = eligibleDates.reduce(function (memo, d) {
        return memo + wagesEarnedOnDate.call(this, d)
    }.bind(this), 0) // <== Hm, why did we need to add bind() there? We'll discuss soon!
    
    return payable
}

let findEmployeeByFirstName = function(src, firstName){
    // console.log(this)
    return src.find(function(emp) {
		return emp.firstName === firstName;
	});
}

let calculatePayroll = function(records){
    let pay = records.map(function(obj) {
		return allWagesFor.call(obj);
	});
	return pay.reduce(function(a, b) {
		return a + b;
	});
}


/*
We're giving you this function. Take a look at it, you might see some usage
that's new and different. That's because we're avoiding a well-known, but
sneaky bug that we'll cover in the next few lessons!

As a result, the lessons for this function will pass *and* it will be available
for you to use if you need it!
*/
let test = ["Gray", "Worm", "Security", 1]
let cRecord = createEmployeeRecord(test)
createTimeInEvent.call(cRecord, "44-03-15 0900")
createTimeOutEvent.call(cRecord, "44-03-15 1100")
hoursWorkedOnDate.call(cRecord, "44-03-15")
wagesEarnedOnDate.call(cRecord, "44-03-15")
```

