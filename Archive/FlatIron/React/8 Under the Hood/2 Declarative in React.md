**Imperative Programming:**

- Explicitly describes the actions a program should take
- Describes *how* a program should go about doing those actions
- example - removing the last element from an array:
  - *access* the element at index arr.length - 1 *from this array* and erase it from memory
  - *resize* the array to have 1 less element at the end
  - *return* to me this array

**Declarative Programming:**

- Describes *what* a program should accomplish (or what the end result should be)
- Leaves the determination of *how* to get to the end result up to the program
- example - removing the last element from an array:
  - I have this array: `[1, 2, 3]`
  - I want an array like that but without the tail element
  - Make it so, computer.

 **Imagine** for a second that we're hiring someone to paint our house:

In an **imperative** world, we'd tell them to open the can of paint, dip their brush in it, and then move the brush in a stroking fashion along the wall. We'd be telling the painter exactly what to do.

In a **declarative** world, we would tell the painter *"I want a house with a big ol' cartoon house horrendously smeared across the side of it...Oh! And I've had a tough week so make my day while doing it..."*, and she'd get it done!

 imagine we have a 'find a hog by weight' component that allows us to filter an array of existing hogs (by weight!).

In our code below (which is a special format that React uses), we don't describe *how* to update the browser (i.e. "remove that `<div>`, add this `<li>`, etc."). Instead, we provide React with a template of *what* the component should look like once it is finished being prepared, i.e.:

```html
<div id="my-hog-world" className="dank-styling">
  { filteredHogsArray.map(hog => "<img src=${hog.img}>") }
  <!-- ^ e.g. show all my hogs in list elements! -->
</div>
```

