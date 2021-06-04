##### Introduce the Anchor Link Tag

An `href` attribute to tell the browser where we want the link to go to.

```html
<a href="https://www.google.com">Google</a>
```

The `href` attribute in this example, is an **full link**, also known as an **absolute** **path**. **Alternatively**, we can also use a ***relative* path**, which is **used** when we want to link to a **separate** **file** on the **same website:**

```html
<a href="about.html">About Page</a>
```

The `target` attribute can be added along side `href` and has a specific use: setting this attribute to be `target='_blank'` will cause the browser to open a new tab when the link is clicked, instead of changing the current page you are on:

```html
<a href="https://www.google.com" target="_blank">Google</a>
```

