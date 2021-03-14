Like indicator Lab

```JS
// Defining text characters for the empty and full hearts for you to use later.
const EMPTY_HEART = '♡';
const FULL_HEART = '♥';
function togLike() {
	let glyph = document.querySelector('.like-glyph');
	glyph.classList.toggle('activated-heart');
	if (glyph.textContent === EMPTY_HEART) {
		glyph.innerHTML = FULL_HEART;
	} else {
		glyph.innerHTML = EMPTY_HEART;
	}
}
// Your JavaScript code goes here!
function likeCall() {
	let error = document.getElementById('modal');
	mimicServerCall()
		.then((s) => {
			togLike();
			console.log(s);
		})
		.catch((e) => {
			console.log(e);
			error.innerText = e;
			error.classList.replace('hidden', 'not');
			setTimeout(function() {
				error.classList.replace('not', 'hidden');
			}, 5000);
		});
}
document.addEventListener('click', likeCall);
//------------------------------------------------------------------------------
// Ignore after this point. Used only for demo purposes
//------------------------------------------------------------------------------

function mimicServerCall(url = 'http://mimicServer.example.com', config = {}) {
	return new Promise(function(resolve, reject) {
		setTimeout(function() {
			let isRandomFailure = Math.random() < 0.2;
			if (isRandomFailure) {
				reject('Random server error. Try again.');
			} else {
				resolve('Pretend remote server notified of action!');
			}
		}, 300);
	});
}

```

