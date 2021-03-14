## Add Toy Info to the Card

Each card should have the following child elements:

- `h2` tag with the toy's name
- `img` tag with the `src` of the toy's image attribute and the class name "toy-avatar"
- `p` tag with how many likes that toy has
- `button` tag with a class "like-btn"

After all of that, the toy card should resemble:

```html
  <div class="card">
  	<h2>Woody</h2>    
    <img src=toy_image_url class="toy-avatar" />    
     <p>4 Likes </p>    
  <buttonclass="like-btn">Like <3</button>  </div>
```

## Add a New Toy

- When a user submits the toy form, a `POST` request is sent to `http://localhost:3000/toys` and the new toy is added to Andy's Toy Collection.
- The toy should conditionally render to the page.
- In order to send a POST request via Fetch, give the Fetch a second argument of an object. This object should specify the method as `POST` and also provide the appropriate headers and the JSON-ified data for the request. If your request isn't working, make sure your header and keys match the documentation.

## Increase Toy's Likes

When a user clicks on a toy's like button, two things should happen:

- Conditional increase to the toy's like count without reloading the page
- A patch request sent to the server at `http://localhost:3000/toys/:id` updating the number of likes that the specific toy has
- Headers and body are provided below (If your request isn't working, make sure your header and keys match the documentation.)

# Lab solution

```JS
let addToy = false;

document.addEventListener('DOMContentLoaded', () => {
	const addBtn = document.querySelector('#new-toy-btn');
	const toyFormContainer = document.querySelector('.container');
	addBtn.addEventListener('click', () => {
		// hide & seek with the form
		addToy = !addToy;
		if (addToy) {
			toyFormContainer.style.display = 'block';
		} else {
			toyFormContainer.style.display = 'none';
		}
		fetch('http://localhost:3000/toys')
			.then(function(response) {
				return response.json();
			})
			.then(function(object) {
				makeToyCards(object);
			});
		const toyForm = document.querySelector('.add-toy-form');
		toyForm.addEventListener('submit', () => {
			event.preventDefault();
			makeNewToy();
		});
	});

	fetch('http://localhost:3000/toys').then((response) => response.json()).then((toyData) => {
		displayCard(toyData);
	});

	function makeNewToy() {
		const nameBtn = document.querySelector('input:nth-child(2)');
		const urlBtn = document.querySelector('input:nth-child(4)');
		fetch('http://localhost:3000/toys', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json',
				Accept: 'application/json'
			},
			body: JSON.stringify({
				name: `${nameBtn.value}`,
				image: `${urlBtn.value}`,
				likes: 0
			})
		})
			.then(function(response) {
				return response.json();
			})
			.then(function(object) {
				console.log(object);
			})
			.catch(function(error) {
				alert('Bad things! Ragnarők!');
				console.log(error.message);
			});
	}

	function displayCard(object) {
		const container = document.getElementById('toy-collection');
		object.forEach((obj) => {
			let div = document.createElement('div');
			let h2 = document.createElement('h2');
			let img = document.createElement('img');
			let p = document.createElement('p');
			let btn = document.createElement('button');
			p.innerText = obj.likes + ' Likes';
			div.className = 'card';
			img.className = 'toy-avatar';
			btn.className = 'like-btn';
			btn.innerText = 'Like <3';
			btn.id = obj.id;
			btn.addEventListener('click', likeToy);
			h2.innerText = obj.name;
			img.src = obj.image;
			container.appendChild(div);
			div.appendChild(h2);
			div.appendChild(img);
			div.appendChild(p);
			div.appendChild(btn);
		});
	}

	function likeToy() {
		event.preventDefault();
		let likes = event.target.previousElementSibling.innerText;
		likes = parseInt(likes) + 1;
		let configObj = {
			method: 'PATCH',
			headers: {
				'Content-Type': 'application/json',
				Accept: 'application/json'
			},
			body: JSON.stringify({
				likes: likes
			})
		};

		fetch(`http://localhost:3000/toys/${event.target.id}`, configObj).then((res) => res.json());
		event.target.previousElementSibling.innerText = `${likes} likes`;
	}
});
```

# Quiz

![image-20200825135737858](C:\Users\camer\AppData\Roaming\Typora\typora-user-images\image-20200825135737858.png)

![image-20200825135802711](C:\Users\camer\AppData\Roaming\Typora\typora-user-images\image-20200825135802711.png)

![image-20200825135909976](C:\Users\camer\AppData\Roaming\Typora\typora-user-images\image-20200825135909976.png)

![image-20200825135957403](C:\Users\camer\AppData\Roaming\Typora\typora-user-images\image-20200825135957403.png)

![image-20200825140024554](C:\Users\camer\AppData\Roaming\Typora\typora-user-images\image-20200825140024554.png)89/