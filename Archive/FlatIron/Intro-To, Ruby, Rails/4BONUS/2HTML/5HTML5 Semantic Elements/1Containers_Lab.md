`span` tag will display without line breaks, whereas content wrapped with `div` *will*:

```html
<span>This content will share the same line...</span><span>...as this content</span>
 
<div>
  This message will appear on a new line
</div>
```

### Semantic Elements

#### `header` and `footer` Tags

The `header` tag is used to wrap all content we would want to contain within the top, (header), portion of a page. The `footer` is for everything at the foot, (bottom), of a page:

```html
<header>
  <!-- Headers often contain company logos -->
</header>
 
<!-- All the main content of a web page goes in between -->
 
<footer>
  <!-- Footers often contain resources, privacy policy links, and copyright information -->
</footer>
```

#### `nav` Tags

**navigation links** to help users acces **different parts of a website**. Wrapping `nav` around links helps describe those links as the page navigation itself:

```html
<nav>
  <a href="about.html">About</a>
  <a href="contact.html">Contact</a>
</nav>
```

#### `<main>` Tag

The `main` tag specifies the ***main*** **content** of a web page

```html
<header></header>
<nav></nav>
 
<main>
  <!-- All the main content of a web page goes here -->
</main>
 
<footer></footer>
```

#### `<section>` Tag

Within the `main` tag breakdown content into specific, meaningful sections

```html
<section>
  <p>Lorem ipsum dolor sit amet...</p>
  <p>Lorem ipsum dolor sit amet...</p>
</section>
```

The `section` tag can be used to define specific portions of a web page. 

#### `article` and `aside` Tags

The `article` tag is for containing written content such as a news story or blog post. The `aside` tag is for containing content that may be related to other content, but is better kept separated.

```html
<article>
  <h1>First Human Digitizes Brain</h1>
  <p>In 2018, Chrome Boi became the first human to digitize their brain. They now live in the Internet.</p>
</article>
 
<aside>
  <h4>Once human, now digital</h4>
  <p>A quick visit to https://en.wikipedia.org/wiki/Draft:Chrome_Boi will show you the ascended individual</p>
</aside>
```

#### `<figure>` and `<figcaption>` Tags

The `figure` tag also comes with a companion for providing captions, the `figcaption` tag. Since `figure` is used for media, the `figcaption` tag can be used to add an additional message about that media or its source.

```html
<section>
 
  <article>
    Lorem ipsum dolor sit amet...
  </article>
 
  <figure>
    <img src="images/intro-pic.jpg"  alt="An exceptional living room." title="Welcome to Exceptional Living Rooms">
    <figcaption>"An Exceptional Living Room" by Leonardo DaVinci, photograph</figcaption>
  </figure>
 
</section>
```

