---
title: "Notes   HTML and CSS"
date: 2021-03-11T14:10:32-08:00
categories:
  - Tech
  - Web
tags:
  - HTML
  - CSS
draft: false
---

## Table
### `colspan` & `rowspan` Examples

```
<table>
  <tr>
    <td>Row 1, Col 1</td>
    <td>Row 1, Col 2</td>
  </tr>
  <tr>
    <td colspan="2">Row 2, Col 1 & Col 2</td>
  </tr>
</table>
```

```
<table>
  <tr>
    <th>Row 1, Col 1</th>
    <th>Row 1, Col 2</th>
  </tr>
  <tr>
    <td>Row 2, Col 1</td>
    <td rowspan="2">Row 2 & Row 3, Col 2</td>
  </tr>
  <tr>
    <td>Row 3, Col 1</td>
  </tr>
</table>
```

## Link
### Jump to a topic
If you want to link to a specific topic/section of a page, Add an `id` attribute and give a name to the section of the page.
```
<body>
    <h2 id="top">My Report</h2>
    <p>
        a very long report
        ...
    </p>
    <div id="footer">
        <a href="#top">Back To Top</a>
    </div>
</body>

```

## Style
### Font Family
* You can use multiple fonts, each separated by comma
* If a font name contains white-space, it must be quoted. Single quotes must be used when using the "style" attribute in HTML.
```
<h1 style="font-family: Jokerman,'Lucida Handwriting',Gigi">User Metrics</h1>
```

## Input
### Date
```html
<input type="date" value="2017-06-01">

<input type="date" id="start" name="trip-start"
       value="2018-07-22"
       min="2018-01-01" max="2018-12-31">
```

The displayed date format will differ from the actual value — 
the displayed date is formatted based on the locale of the user's browser, 
but the parsed value is always formatted `yyyy-mm-dd`.