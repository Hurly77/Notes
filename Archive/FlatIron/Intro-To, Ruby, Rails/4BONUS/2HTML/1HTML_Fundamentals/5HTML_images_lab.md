### **HTML Images**

Images are inserted into HTML using the `img` tag. The `img` tag **is *self closing*,**There are two main attributes, `src`, the *source* of the image, and `alt`, the *alternate* text.

The `src` attribute provides the relative path or URL to the image file we want to display

```html
<img src="../images/my_company_logo.png">
```

However, it's very common, even when publishing your own websites, to have images hosted somewhere else on the internet

```html
<img src="https://i.imgur.com/H1qsYEl.png">
```

The `alt` attribute contains text relevant to the image we're displaying, and will appear in its place if the image fails to load.

To include an `alt` attribute, add it in along with the `src` attribute:

The `alt` text is **important** for screen readers for the **visually impaired**, as the text will be read out loud to the site visitor. It is also nice to provide some sort of **message** to a website visitor **if** the **image fails to load**

```html
<img src="https://i.imgur.com/H1qsYEl.png" alt="comedic crow gets wholesome support">
```

One additional attribute that can be useful is the `title`. Content added to this attribute will display when we hover over the image

```html
<img src="../images/my_company_logo.png" alt="my company name" title="We're here to help you!">
```

