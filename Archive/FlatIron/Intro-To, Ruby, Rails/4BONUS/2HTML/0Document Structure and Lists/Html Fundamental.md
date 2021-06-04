### Identify ordered, unordered and definition lists

```html
#HTML unordered list, represented by the the ul tag.
<ul>
  <li>One item</li>
  <li>Another item</li>
</ul>
    #we use an ordered list, or the `ol` tag
 <ol>
  <li>First item</li>
  <li>Second item</li>
</ol>
#Each li is a list item contained in the larger ul or ol container.
```

Another type of list we can use is a definition list, which defines specific types of items.

```html
<dl>
  <dt>First term</dt>
  <dd>Term definition</dd>
</dl>
```



### Identify images

To include an image in our page, we use an `img` tag. Our `alt` attribute descriptive text the browser if it can't find the image file display the `title` text The `width` and `height` attributes define the size

```html
<img src="myimage.jpg" alt="Alternative Text" title="Display Title"
width="800" height="600">
#This tag closes itself.
```



### Identify links

We begin with a standard text hyperlink.

```html
<a href="http://example.com/">This is a link</a>

##if we want to link an image instead of text

<a href="http://example.com/">
  <img src="myimage.jpg" alt="Alternative Text">
</a>

#if we want a link that will direct to an email address
<a href="mailto:webmaster@example.com">Send an email</a>

##to link to another, specific location on the same webpage

<p id="tips">Useful Tips Section</p>
<a href="#tips">Jump to the Useful Tips Section</a>
```

A relative link directs to content within the same website.

```html
<a href="about.html">This is a relative URL link</a>
```

An absolute link links to external content

```html
<a href="http://example.com/">This is an absolute URL link</a>
```

