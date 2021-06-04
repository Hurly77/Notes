## **CSS Fundamentals**

#### Identify CSS Syntax

![CSS syntax diagram labeling selector, property and value](https://curriculum-content.s3.amazonaws.com/fewds/css-syntax.png)

#### Identify CSS Use Formats

inline, internal (or embedded) and external.

**Inline** - includes the styles directly into the HTML element with the `style` attribute.

```html
<p style="color: blue;"></p>
```

 **NOT best practice for two reasons.**, it only affects that single element  **and** breaks our principle of separation of content and presentation

`Internal CSS` - is **inside** of `style` tags in the HTML document's `head` **section**.

```HTML
<!DOCTYPE html>
<html>
  <head>
    <style>
      p { color: blue; }
    </style>
  </head>
  <body>
 
  <p>This is a paragraph.</p>
 
  </body>
</html>
```

This CSS will **only** **apply** to the **single** **document**. 

**external** - CSS to carry across various pages

```HTML
<!DOCTYPE html>
<html>
  <head>
    <link rel="stylesheet" href="styles.css">
  </head>
  <body>
 
  <p>This is a paragraph.</p>
 
  </body>
</html>
```

With the `link` tag, we can use the relation attribute to define the target as a stylesheet and the link source our CSS file

##### ID and Class Selectors

**ID selectors** target all elements with a specific ID

 CSS rule is to **follow** the **element** **name** with a **hash** **tag** and then the **ID** **attribute** bwe want to **match**.

```css
p#introduction {
  color: blue;
}
```

**Class selectors** 

 target all elements with a class attribute value matching the selector name

```css
p.alert {
  color: red;
}
```

The **difference** between **IDs** and **classes** is that **IDs** are **meant** for **one** **element** on the page that has **unique** identity where **class** **selectors** are **meant** to be **spread** **throughout** the page across multiple elements.

##### Compound Selectors

**Compound** selectors let us apply the same CSS rules to **multiple** elements at once.

```css
h1, h2 {
  color: green;
}
```

This **eliminates** the need to rewrite a new **CSS rule** containing the same styles for different elements.

##### Descendant Selectors

**Descendant** selectors **target** elements that are descendants of the **matching** **selector name.**

```css
article p {
  color: blue;
}/*indicated by a space in between one selector and another selector.
```



##### Child Selectors

The **child selector** targets **all** elements that are the **immediate** **children** of a specified element.

```css
article > p {
  color: blue;
}
```

##### Adjacent Sibling Selector

The **adjacent sibling** selector **targets** elements that appear **directly** **after** the matching selector name. We **indicate** it using a **plus** symbol.

```css
h3 + p {
  color: blue;
}
```

##### General Sibling Selector

The **general sibling** selector (sometimes called the preceded selector) will style **all** matched elements **after** the **preceeding selector** name.

```css
h3 ~ p {
  color: red;
}
```



##### Universal

The **universal** selector matches **any** elements and will apply to elements that are **not** **targeted** by other rules. It's indicated by the star symbol.

```css
* {
  color: yellow;
}
```

##### Attribute Selectors

The `attribute` selector can target elements with a **particular** **attribute**. We can also define **exactly** which **attribute** we want to match.

```css
input[type="text"] {
  width: 200px;
}
```

##### Pseudo-class Selectors

**Pseudo-class** selectors target elements **based** on a particular **state** of an element or **relationship** to other **elements**. The way we signify a pseudo class selector is with the colon symbol.

```css
a:link {
  color: blue;
}
 
a:visited {
  color: purple;
}
```

## Utilize Various Types of Color Values in CSS

##### Hexidecimal Color Values

Hex colors begin with `#` and are followed by, generally, 6 numbers

```
Decimal Numbers:      0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16
Hexadecimal Numbers:  0, 1, 2, 3, 4, 5, 6, 7, 8, 9,  a,  b,  c,  d,  e,  f, 10
```

Hex colors work by creating Red, Green, Blue (RGB) values. Traditional RGB colors are on a scale of 0 to 255 for each of the three colors in the spectrum.`#000000` translates to black since 0 reds, 0 green, 0 blues represents the absence of all colors, and `#ffffff` makes white since 255 reds, 255 greens, and 255 blues equal the maximum of each of the colors.

Hex colors can be shortened to just three numbers when each RGB value is the same for each digit. So `#111111` can be written as `#111`.

#### RGB Color Values

```css
p {
  color: rgb(255, 255, 255);
}
```

You can also add an extra channel to your RGB color by setting an "a" value, which represents opacity.

```css
p {
  color: rgba(0, 0, 255, 0.5);
}
```

#### CSS Comments

```css
p.alert {
  color: #ff0000; /* Alert text displays red */
}
```

`/* */`