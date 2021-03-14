## Describe How `iframe` Elements Work

The `iframe` creates a window inside the page where this "shared" information appears. An `iframe`'s `src` attribute points to the location of the shared material. Examples are a custom search bar or YouTube video.

```html
<iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d335994.89219194185!2d2.0673752159642937!3d48.8589713267984!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x47e66e1f06e2b70f%3A0x40b82c3688c9460!2sParis%2C+France!5e0!3m2!1sen!2sus!4v1457911182825" width="600" height="450" frameborder="0" style="border:0" allowfullscreen></iframe>
```



<iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d335994.89219194185!2d2.0673752159642937!3d48.8589713267984!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x47e66e1f06e2b70f%3A0x40b82c3688c9460!2sParis%2C+France!5e0!3m2!1sen!2sus!4v1457911182825" width="600" height="450" frameborder="0" style="border:0" allowfullscreen></iframe>

#### Some Important Iframe Attributes

##### `src`

The `iframe` element has one required attribute: `src`. The `src` attribute takes a URL (`http://example.com/....`) and displays the page requested.

##### `width` and `height`

`width` and `height` allow us to control the size of the `iframe` that we'd like to display. Depending on the website that you are using in your `iframe`, it might have a size built in, but to be safe you always want to specify a size. It's worth noting that if you know CSS, you can control height and width there as well.

##### `frameborder` and `style`

The `frameborder` attribute is considered *deprecated*, meaning "likely to be removed from the standard." In modern browsers, we can control borders using CSS, as with our example, `style="border:0"`. If you do, you'll want to set both `frameborder="0"` AND `style="border:0"`.

##### `allowfullscreen`

 The `allowfullscreen` attribute uses a JavaScript method called `requestFullScreen()` to send the `iframe` to full screen.