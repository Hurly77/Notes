# Babel

babel allows your to use a less repetitive syntax with **JSX** ex:

```jsx
var profile = (
  <div>
    <img src="avatar.png" className="profile" />
    <h3>{[user.firstName, user.lastName].join(' ')}</h3>
  </div>
)
```

...when the above is run through Babel, we receive the following executable code:

```jsx
var profile = (
  React.createElement("div", null,
  React.createElement("img", { src: "avatar.png", className: "profile" }),
  React.createElement("h3", null, [user.firstName, user.lastName].join(" ")))
);
```

*JSX code in the first block, (which looks like some abomination between HTML and plain JavaScript), was transformed into valid JavaScript syntax in the second block after Babel had a go at it.*

While you don't **strictly** need Babel as a dependency when writing React code, not having it means you have to write in the non-JSX syntax seen in the output above