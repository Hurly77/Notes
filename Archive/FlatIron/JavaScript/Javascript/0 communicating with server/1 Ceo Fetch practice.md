## Challenge 1

This repository includes an `index.html` file that loads an `index.js` file.

```
const imgUrl = "https://dog.ceo/api/breeds/image/random/4"
```

Add JavaScript so that:

- on page load
- fetch the images using the url above ⬆️
- parse the response as `JSON`
- add image elements to the DOM **for each**🤔 image in the array

------

## Challenge 2

```
const breedUrl = 'https://dog.ceo/api/breeds/list/all'
```

After the first challenge is completed, add JavaScript so that:

- on page load, fetch all the dog breeds using the url above ⬆️
- add the breeds to the page in an `<ul>` (take a look at the included `index.html`)

------

## Challenge 3

Once all of the breeds are rendered in the `<ul>`, add JavaScript so that the font color of a particular `<li>` changes *on click*. This can be a color of your choosing.

When the user clicks any of the dog breed list items, the color the text should change.

------

## Challenge 4

Once we are able to load *all* of the dog breeds onto the page, add JavaScript so that the user can filter breeds that start with a particular letter using a dropdown.

For example, if the user selects 'a' in the dropdown, only show the breeds with names that start with the letter a. For simplicity, the dropdown only includes the

# lab solution\

```js
console.log('%c HI', 'color: firebrick');

const imgUrl = 'https://dog.ceo/api/breeds/image/random/4';
const breedUrl = 'https://dog.ceo/api/breeds/list/all';

document.addEventListener('DOMContentLoaded', function() {
  
  let allBreeds = []
  const dropDown = document.getElementById("breed-dropdown");
  const ul = document.getElementById('dog-breeds');
  const li = document.createElement('li');
  const div = document.getElementById('dog-image-container');
  ul.style.cursor = 'pointer';
  
  fetch(imgUrl).then((response) => response.json()).then((json) => renderImg(json['message']));
  fetch(breedUrl).then((response) => response.json()).then((breedData) => {
    allBreeds = Object.keys(breedData.message)
      ul.innerHTML = dogBreedList(allBreeds)
  });
  
  dropDown.addEventListener('change', function (event) {
    firstLetter = event.target.value
    const filterBreed = allBreeds.filter((breed) => breed.startsWith(firstLetter)) 
    ul.innerHTML = dogBreedList(filterBreed)
  });

  ul.addEventListener('click', function(event){
    let color = '#' + ((Math.random() * (1 << 24)) | 0).toString(16);
    event.target.style.color = color;
  })

  function renderImg(urls) {
    urls.forEach((url) => {
      const div2 = document.createElement('div'),
        img = document.createElement('IMG');
      (img.src = url), (div2.id = 'gallery');
      div2.appendChild(img), div.appendChild(div2);
    });
  }

  function dogBreedList(breedList){
    const liArray = breedList.map(function(breed){
      return `<li>${breed}</li>`
    })
    return liArray.join('')
  }
});
```

