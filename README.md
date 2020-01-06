# test-site

Note:
1. Troubleshoting and cross-browser support
For browsers that don't support SVG (IE 8 and below, Android 2.3 and below), you could reference a PNG or JPG from your src attribute and use a srcset attribute (which only recent browsers recognize) to reference the SVG. This being the case, only supporting browsers will load the SVG — older browsers will load the PNG instead:
```
<img src="equilateral.png" alt="triangle with equal sides" srcset="equilateral.svg">
```
