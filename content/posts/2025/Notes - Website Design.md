---
title: "Notes - Website Design"
date: 2025-01-17T08:22:33-08:00
categories:
  - Tech
  - Web
tags:
  - Hugo
  - Blog
  - Website
draft: false
---

## Favicon
A favicon is a small image displayed next to the page title in the browser tab.

Tip: 
* A favicon is a small image, so it should be a simple image with high contrast. 
* You can generate the icons and codes by following the instruction from [Favicon generator](https://realfavicongenerator.net/).

The most common favicon formats are ICO, PNG, and SVG, JPEG, GIF. 

To add a favicon to your website, either save your favicon image to the root directory of your webserver, or create a folder in the root directory called images, and save your favicon image in this folder. A common name for a favicon image is "favicon.ico".

Next, add a <link> element to your "index.html" file, after the <title> element, like this:

```
<link rel="icon" href="https://example.com/image.ico">
<link rel="icon" type="image/gif" href="https://example.com/image.gif">
<link rel="icon" type="image/png" href="https://example.com/image.png">
// All browser except Safari
<link rel="icon" type="image/svg+xml" href="https://example.com/image.svg">
// For Safari only
<link rel="mask-icon" href="https://example.com/image.svg" color="red">
```


