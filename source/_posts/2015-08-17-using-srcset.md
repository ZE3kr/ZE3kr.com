---
title: Use srcset + sizes attributes and w identifier to solve all responsive image problems
tags:
  - HTML5
  - Responsive Design
id: '4019'
categories:
  - - Development
date: 2015-08-17 09:07:34
languages:
  zh-CN: https://guozeyu.com/2015/08/using-srcset/
---

Using the srcset attribute can solve all responsive image problems, but there are two cases here, photos of certain and indeterminate widths. In contrast, photos of indeterminate width are more complicated.

## Determine the Width of the Photo

A fixed-width photo here refers to a photo whose style attribute width is set to a fixed number of px. As more and more high device pixel ratio (referred to as device pixel ratio, the same below) displays appear, websites need higher pixel photos to fit these displays. For example, there is a photo with a display width of 200px, which occupies 200 physical pixels (that is, the actual occupied pixels, the same below); it actually occupies 400 physical pixels on a @2x display; it actually occupies 600 physical pixels on a @3x display; similarly, on a @4x display That's 800 physical pixels. If this photo was only available in 200px, it would look blurry on @2x\~@4x monitors. If only the 800px version is available, it will load unnecessary content on @1x\~@3x devices, which means longer unnecessary time (especially on mobile phones). At this point, you need to use the responsive image method. The easiest and most efficient way is to use srcset to solve it. For example, 4 photos with widths of 200, 400, 600, and 800 pixels are now provided. Through the srcset attribute, these 4 photos can be displayed on monitors with a device pixel ratio of @1x~@4x, so that on each monitor It can display the best photo for it. The file names of these four pictures are 200px.png, 400px.png, 600px.png, 800px.png respectively. To sum up, the physical pixels occupied by the photo to determine the width are only related to the device pixel ratio, so only the x identifier of the srcset attribute is needed.

```
<img src="200px.png" srcset="400px.png 2x, 600px.png 3x, 800px.png 4x">
```

Adding the srcset attribute in this way, the browser will load different photos according to the pixel ratio of its own device. Browsers that do not support the srcset attribute will load the photo 200px.png by default. This method is backward compatible.

## Photos of Indeterminate Width

The indeterminate width photo here refers to the photo whose style attribute width is set to a certain percentage. If it is a photo with an uncertain width (or the width of the image is uncertain on the mobile phone, the width is determined on the computer, etc.), it can also be solved by srcset. Since the pixels cannot be determined, it is difficult to achieve that the photo itself and the physical pixels occupied by the photo are the same, because the user's window size is indeterminate. If you only prepare 5 photos, the width is 400~3200px, and the file names are 400px.png, 800px.png, 1600px, 2400px, 3200px. Usually, if there is no photo corresponding to the physical pixels occupied by the image, an image that is slightly larger than the physical pixels occupied by the image will be loaded. For example, if the photo occupies 500 physical pixels of the screen, then you should choose to load 800px.png. Loading slightly larger photos in this way can still achieve the best display effect, while saving network resources to the greatest extent. To sum up, the physical pixels occupied by a photo with an uncertain width are related to the device pixel ratio and the occupied width. However, after using the w identifier of the srcset attribute, you only need to specify the width, and the browser will automatically calculate the pixel ratio according to the device pixel ratio. Choose the best picture. If the image occupies 100% of the entire window size (ie 100vw width, the same below), then you only need to set srcset in this way to achieve the effect. Putting aside the concept of device pixel ratio, the w identifier of this new srcset attribute allows the browser to adapt itself to the photo to load.

```
<img src="800px.png" srcset="400px.png 400w, 800px.png 800w, 1600px.png 1600w, 2400px.png 2400w, 3200px.png 3200w" alt="" />
```

However, there is still a small problem at this time. The browser is relatively stupid. It will think that the width of the image is always 100% of the width of the entire window. What if the image does not occupy 100% of the entire window and there are borders on both sides? In this case, you need to use the size attribute. The width of the image is specified in the sizes attribute. The percentage unit cannot be used, but units such as vw and px can be used (100vw is the width of the entire window), or calc operation can be used, and media queries are supported. To ensure that the width calculated in sizes is always the width of the screen occupied by the image, the rest only needs to be done by the browser. Here are some typical case examples:

### The Borders on Both Sides are Percentages

If the borders on both sides of the picture (referring to the length from the edge of the picture to the edge of the window) always occupy a certain percentage of the screen, then the picture will always have a percentage relative to the entire window. For example, the borders on both sides of the picture are always 10% of the entire window, then the size property can be set as follows:

```
sizes="80vw"
```

If the image is already under an element whose width is 70% of the entire window, and there are still 10% borders on both sides of the image within the element, then the size property can be set as follows:

```
sizes="50vw"
```

### The Borders on Both Sides are Determined Pixels

For example, the borders on both sides of the image are always 10px, then the size property can be set as follows:

```
sizes="calc( 100vw - 10px )"
```

If the image is already under an element whose width is 70% of the entire window, and the borders on both sides of the image inside the element are always 10p, then the size property can be set as follows:

```
sizes="calc( 70vw - 10px )"
```

### Image Has Maximum Size

For example, the borders on both sides of the picture are always 10px, but the maximum size of the picture can only be 1000px, then the size property can be set as follows:

```
sizes="(min-width: 1020px) 1000px, calc( 100vw - 10px )"
```

Where `(min-width: 1020px) 1000px` means that when the screen pixel width is greater than 1020px, the image width is 1000px.

### Actual Effect

Take my website as an example here. The frame of the picture on my website is 1.875rem. The picture itself is under an element. The default width of this element is 100% of the entire window, but when the screen width is greater than 40.063rem, this The element occupies 50% of the entire window, and this element has a maximum size of 500px. Now put the size attribute together with the srcset attribute with the w identifier, and it's perfectly solved.

```
<img src="800px.png" sizes="(min-width: 1000px) calc( 500px - 1.875rem ), (min-width: 40.063rem) calc( 50vw - 1.875rem ), calc( 100vw - 1.875rem ) " srcset="400px.png 400w, 800px.png 800w, 1600px.png 1600w, 2400px.png 2400w, 3200px.png 3200w" alt="" />
```

Then you can test it in the browser to achieve the effect of adapting to all screen sizes and pixel ratios of all devices.

### About Compatibility

In general, the x identifier of the srcset attribute is supported better than the w identifier, because the w identifier is a new attribute. But the good news is that Chrome 38+, Firefox 38+, Safari 9+, Opera 30+, Android 5.0+, iOS 9+ (yes, IE/Edge, Windows Phone are not supported as of the date of writing) are all supported The w identifier has been removed, so I think it is now a good time to use the w identifier of the srcset attribute for responsive images. And this method is perfectly backward compatible, even if it is not supported. You can also [check on CanIUse.com](http://caniuse.com/#feat=srcset) for a complete compatibility list for the secset attribute.
