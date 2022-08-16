---
title: "Notes - Bootstrap"
date: 2022-08-06T10:33:40-07:00
categories:
  - Web
tags:
  - Bootstrap
  - HTML
  - CSS
draft: false
---

## Table
### Horizontal Alignment - Middle
* Margin: `mx-auto`
* Text: `text-center`
* Flex: `d-flex justify-content-center`, `align-self-center`


### Vertical Alignment - Middle
* [Vertical Alignment Utitlities](https://getbootstrap.com/docs/4.6/utilities/vertical-align/):
Change the vertical alignment of inline, inline-block, inline-table, and table cell elements.
```
<span class="align-middle"> ... </span>
<td class="align-middle"> ... </td>
```

* [Flex Utilities](https://getbootstrap.com/docs/4.6/utilities/flex/#enable-flex-behaviors):
To change the alignment of grid columns, navigation, components, and more
```
<div class="d-flex align-items-center">...</div>
```

### Table Caption
Default is at the bottom of a table. 
Use `.caption-top` to change the position at the top of the table.
```
<table class="table table-striped caption-top">
    <caption>研习会详细时间表</caption>
    <thead>
    <tr>
        <th colspan="2">10月21日(星期五)</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>19:00 - 19:30</td>
        <td>开幕与敬拜</td>
    </tr>
    <tr>
        <td>19:30 - 20:45</td>
        <td>以佈道为导向的教会</td>
    </tr>
    </tbody>
</table>
```

## Card
### How to make the card the same height in card-column?
For responsive cards, it may show one card per row on cell phone, but 4 cards per row on desktop.
It's difficult to make the contents the same in every card. 

Use class `h-100` to make all the cards the same height of their parent column:
```
<div class="col-12 col-sm-6 col-lg-4 my-3">
    <div class="card h-100">
    ...
    </div>
</div>
```

## Background
### Background Color
* div
```
<div class="row my-5" style="background-color:rgba(241,216,142,0.6);">
```

* nav - Bootstrap 4
```
<nav class="navbar navbar-light" style="background-color: #e3f2fd;">
  <!-- Navbar content -->
</nav>
```
### Background Image
* Whole page
```
<style>
body {
    background-image: url('static/bg2.jpg');
    background-repeat: no-repeat;
    background-attachment: fixed;
    background-size: 120% 80%;
}
</style>
```

* div
```
<div class="container" style="background-image: url(/static/pentecost/bg2.jpg); background-size: 100% 100%;background-repeat: no-repeat;background-attachment: fixed;">
  ...
</div
```

## List
### Un-styled
```
<ul class="list-unstyled">
  <li>Not like a list</li>
</ul>  
```

## Embed
Responsively handling video or slideshow embeds based on the width of the parent.
* For V4.6
```
<div class="embed-responsive embed-responsive-16by9">
  <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/zpOULjyy-n8?rel=0" allowfullscreen></iframe>
</div>
```
* For V5.2
```
<div class="ratio ratio-16x9">
  <iframe src="https://www.youtube.com/embed/zpOULjyy-n8?rel=0" title="YouTube video" allowfullscreen></iframe>
</div>
```

## Useful Links
* [Partial Collapse](https://stackoverflow.com/questions/52332952/can-we-use-partial-collapse-concept-in-bootstrap-4)
* 