### How to Embed Audio Elements in HTML5

##### `<audio>`

To include audio in a website `<source>` elements whose`src` attributes point to a file on the server`type` attributes specify what type of media it is .A few **examples** are `text/html`, `text/css`, `images/jpeg`.

```html
<audio controls>
  <source src="purrr.mp3" type="audio/mp3">
  <source src="purrr.ogg" type="audio/ogg">
  <p>Sorry your browser doesn't support HTML5 Audio! Please <a href="http://browsehappy.com/?locale=en">upgrade your browser</a>.</p>
</audio>
```

`controls` This is required to display the audio controls to start and pause playback. The presence of the `controls` attribute name itself is sufficient, no other properties are needed.

**M**ultipurpose **I**nternet **M**ail **E**xtensions or **MIME**

**optional attributes** - `autoplay` and `loop`. 

#### How To Embed Video Elements in HTML5

the `<video>` tag are are **source** tags that **point** to the **location** of various **video** file formats **and** specify their **MIME** types.tags that point to the location of various video file formats and specify their **MIME** types.

```HTML
<video controls>
  <source src="real-estate.mp4" type="video/mp4">
  <source src="real-estate.ogv" type="video/ogg">
  <p>Sorry your browser doesn't support HTML5 Video! Please <a href="http://browsehappy.com/?locale=en">upgrade your browser</a>.</p>
</video>
```

Like `<audio>`, we will open the `<video>` tag with the controls attribute. 