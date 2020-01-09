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



2.2) **Resolution switching: Same size, different resolutions**
 
 
 
2.3) **Art direction**



2.4) **Summary**

As a recap, there are two distinct problems we've been discussing here:

* **Art direction**: The problem whereby you want to serve cropped images for different layouts — for example a landscape image showing a full scene for a desktop layout, and a portrait image showing the main subject zoomed in for a mobile layout. You can solve this problem using the `<picture>` element.

* **Resolution switching**: The problem whereby you want to serve smaller image files to narrow screen devices, as they don't need huge images like desktop displays do — and also optionally that you want to serve different resolution images to high density/low density screens. You can solve this problem by using vector graphics (SVG images) and the `srcset` with `sizes` attributes.


**Sources**
1. https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Adding_vector_graphics_to_the_Web
2. https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images
