# test-site

Note:
1. **Troubleshoting and cross-browser support**

For browsers that don't support SVG (IE 8 and below, Android 2.3 and below), you could reference a PNG or JPG from your src attribute and use a srcset attribute (which only recent browsers recognize) to reference the SVG. This being the case, only supporting browsers will load the SVG — older browsers will load the PNG instead:
```
<img src="equilateral.png" alt="triangle with equal sides" srcset="equilateral.svg">
```

2. **How do you create responsive images?**

* *Responsive image technologies* were implemented recently to solve the problems indicated above by letting you offer the browser several image files, either all showing the same thing but containing different numbers of pixels (*resolution switching*), or different images suitable for different space allocations (*art direction*).
 
2.1) **Resolution switching: Different sizes**

The `<img>` element only lets you point the browser to a single source file:
`<img src="elva-fairy-800w.jpg" alt="Elva dressed as a fairy">
`
But you can use two new attributes — `srcset` and `sizes` — to provide several additional source images along with hints to help the browser pick the right one:
```
<img srcset="elva-fairy-480w.jpg 480w,
             elva-fairy-800w.jpg 800w"
     sizes="(max-width: 600px) 480px,
            800px"
     src="elva-fairy-800w.jpg" alt="Elva dressed as a fairy">
```
**`srcset`** defines the set of images we will allow the browser to choose between, and what size each image is. Each set of image information is separated from the previous one by a comma. For each one, we write:
 1. An image filename (elva-fairy-480w.jpg)
 2. A space
 3. The image's intrinsic width in pixels (480w) — note that this uses the w unit, not px as you might expect. This is the image's real size, which can be found by inspecting the image file on your computer

**`sizes`** defines a set of media conditions (e.g. screen widths) and indicates what image size would be best to choose, when certain media conditions are true — these are the hints we talked about earlier. In this case, before each comma we write:

 1. A **media condition** ((max-width:600px)) — you'll learn more about these in the CSS topic, but for now let's just say that a media condition describes a possible state that the screen can be in. In this case, we are saying "when the viewport width is 600 pixels or less".
 2. A space
 3. The width of the slot the image will fill when the media condition is true (480px)
    * Notice that the last slot width has no media condition (this is the default that is chosen when none of the media conditions are true). The browser ignores everything after the first matching condition, so be careful how you order the media conditions.

2.2) **Resolution switching: Same size, different resolutions**
 
 If you're supporting multiple display resolutions, but everyone sees your image at the same real-world size on the screen, you can allow the browser to choose an appropriate resolution image by using `srcset` with x-descriptors and without `sizes`:
 
 ```
 <img srcset="elva-fairy-320w.jpg,
             elva-fairy-480w.jpg 1.5x,
             elva-fairy-640w.jpg 2x"
     src="elva-fairy-640w.jpg" alt="Elva dressed as a fairy">
 ```
 
 For example, the following CSS is applied to the image so that it will have a width of 320 pixels on the screen:
 ```
 img {
  width: 320px;
}
 ```
In this case, `sizes` is not needed — the browser simply works out what resolution the display is that it is being shown on, and serves the most appropriate image referenced in the `srcset`. So if the device accessing the page has a standard/low resolution display, with one device pixel representing each CSS pixel, the `elva-fairy-320w.jpg` image will be loaded (the 1x is implied, so you don't need to include it.) If the device has a high resolution of two device pixels per CSS pixel or more, the `elva-fairy-640w.jpg` image will be loaded. 
 
 
2.3) **Art direction**
**Art direction** involves wanting to change the image displayed to suit different image display sizes. For example we would use the `<picture>` element to show a smaller portriate on mobile so the image does not shrink down and make the image unviewable.

In this example we have an image that badly needs art direction:
```
<img src="elva-800w.jpg" alt="Chris standing up holding his daughter Elva">
```
Let's fix this, with `<picture>`! Like `<video>` and `<audio>`, the `<picture>` element is a wrapper containing several `<source>` elements that provide different sources for the browser to choose from, followed by the all-important `<img>` element. Our example code looks like so:
```
<picture>
  <source media="(max-width: 799px)" srcset="elva-480w-close-portrait.jpg">
  <source media="(min-width: 800px)" srcset="elva-800w.jpg">
  <img src="elva-800w.jpg" alt="Chris standing up holding his daughter Elva">
</picture>
```
 * The `<source>` elements include a `media` attribute that contains a media condition — as with the first `srcset` example, these conditions are tests that decide which image is shown — the first one that returns true will be displayed. In this case, if the viewport width is 799px wide or less, the first `<source>` element's image will be displayed. If the viewport width is 800px or more, it'll be the second one.
 * The `srcset` attributes contain the path to the image to display. Just as we saw with `<img>` above, `<source>` can take a `srcset` attribute with multiple images referenced, as well as a `sizes` attribute. So, you could offer multiple images via a `<picture>` element, but then also offer multiple resolutions of each one. Realistically, you probably won't want to do this kind of thing very often.
 * In all cases, you must provide an `<img>` element, with `src` and `alt`, right before `</picture>`, otherwise no images will appear. This provides a default case that will apply when none of the media conditions return true (you could actually remove the second `<source>` element in this example), and a fallback for browsers that don't support the `<picture>` element.


2.4) **Summary**

As a recap, there are two distinct problems we've been discussing here:

* **Art direction**: The problem whereby you want to serve cropped images for different layouts — for example a landscape image showing a full scene for a desktop layout, and a portrait image showing the main subject zoomed in for a mobile layout. You can solve this problem using the `<picture>` element.

* **Resolution switching**: The problem whereby you want to serve smaller image files to narrow screen devices, as they don't need huge images like desktop displays do — and also optionally that you want to serve different resolution images to high density/low density screens. You can solve this problem by using vector graphics (SVG images) and the `srcset` with `sizes` attributes.


**Sources**
1. https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Adding_vector_graphics_to_the_Web
2. https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images
