## Getting Started

to incorporate package.json You’ll have to do something like this in your index.html file

```html
<script src="./node_modules/moment/moment.js"></script>
```

Reading from the `src` value, this script is expecting a folder called `node_modules` in the same directory as the HTML file. Inside `node_modules` is another folder, `moment` which contains `moment.js`.

there is another `script tag`

```html
<script src="index.js"></script>
```

This will link you JavaScript to HTML

### Create a `package.json` File

The `package.json` can be written quickly from scratch, but we actually have a handy command for creating these files: `npm init`. Most prompts will provide a default value

### Add a Script

Open the newly created `package.json` file and look for a section titled `"scripts"`. Let's replace the default `"test"` script with an shell command:

```json
"scripts": {
  "test": "echo 'Hello World!'"
}
```

o install a package and save it to your `package.json` file, run `npm install` followed by the package name. In our case, that would be:

```
npm install moment
```

To submit your work in this lesson, you'll have to run `cd ..`