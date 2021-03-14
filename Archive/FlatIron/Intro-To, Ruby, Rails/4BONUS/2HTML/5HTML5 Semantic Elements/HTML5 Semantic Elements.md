## Semantic Elements

HTML authors thought that this was a good idea and an informal standard sprang up around adding `id` **attributes** on elements to express their "**semantic** **meaning**."

```html
<div id="header">
  <div class="wrapper">...</div>
</div>
```

We once used to have to identify a `div` as our header section.

```html
<div id="header">...</div>
```

Now we use the `header` element.

```html
<header></header>
```

Why do we call these *semantic* elements? Semantic elements are elements that we use when the content within the element all has the same related *meaning*. In our `header` example above, all the content we would put within the `header` element would relate to introductory content, such as titles or navigation.

#### HTML5 Semantic Element Use

**This is the markup we begin with:**

```html
<div class="wrapper">
  <div id="header">
     <div id="nav">...</div>
  </div>
  <div id="main">
    <div id="music">
      <div id="rock">...</div>
      <div id="jazz">...</div>
    </div>
  </div>
  <div id="aside">...</div>
  <div id="footer">...</div>
</div>
```

Now we'll **replace** each instance of a `div` with a **semantic** **element** that **matches** the **type** of content we want it to **contain**.

```html
<div class="wrapper">
  <header>
     <nav>...</nav>
  </header>
  <main>
    <section id="music">
      <article id="rock">...</article>
      <article id="jazz">...</article>
    </section>
  </main>
  <aside>...</aside>
  <footer>...</footer>
</div>
```

we can still use `div` elements as we please