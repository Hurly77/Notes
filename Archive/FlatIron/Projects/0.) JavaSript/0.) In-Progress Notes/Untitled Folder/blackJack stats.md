```js

let fc = (n) => {
	return n != 1 ? n * fc(n - 1) : 1;
};

let invFc = (n, f) => {
	if (n / f < 1) {return 'no inverse fact';}
	return n / f != 1 ? invFc(n / f, f + 1) : f;
};

let nCr = (n, r) => {
	return fc(n) / fc(n - r) / fc(r);
}

let add2 = (a) => {
	let a1 = [...a],
			a2 = [...a1],
			count = 0;
	for (const ind of a1) {
		for (let i = 0; i < a2.length; i++) {
			(a1[ind] + a2[i] == 21) ? (count += 1) : 0;
			a1.shift();
		}
	}
	console.log()
	return count;
};

let array = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]

let myR = (s, n, a) => {
	let n2 = a.slice(s, n)
	return n2.reduce((a, b) => (a + b))
}
```

