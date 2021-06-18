### `<!DOCTYPE html>`

 In the **early** days of the **internet**, there were fewer **standards**, and it was **important** to declare the specific way we wanted browsers to **interpret** the **file**

**These days**, every current browser is compatible with HTML5, and `doctype` is mainly **used** to tell the browser **to render the page in standards compliant mode**.

### `<html>`

The next element is also always required: `<html>`. This tells the browser that everything that falls between the opening and closing `html` tags is to be interpreted as HTML code.

```html
<html lang="en">
</html>
<!-- lang="en" this is so search engines know what language a page is written in -->
```

In the `head` section, we place a number of specific tags, most notably:

- <link>

- <title>

**CAREFUL**: It's easy to get confused here because web pages are full of links, but also use a `` tag. "Links" that you click on are located within the `` element. The `` tags are located in the `` element.

```html
<link rel="stylesheet" type="text/css" href="style.css">
```

**Linking** stylesheets this way **allows** multi-page websites **to** **share** a **source** of styling **content** for every page, making for a consistent, **easy** to maintain **file structure**.`definitions like:` 

```html
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/css/bootstrap.min.css">
<link rel="stylesheet" type="text/css" href="company.css">
<link rel="stylesheet" type="text/css" href="engineering-department.css">
<link rel="stylesheet" type="text/css" href="project-x-launch.css">
<link rel="stylesheet" type="text/css" href="typography.css">
```

##### `title`

One more common tag we find in the `head` is `title`. The `title`, as its name implies, is where the title of the webpage should be entered

```html
<title>Cat Perry's Favorite Cats</title>
```

