---
title: "Notes - Bootstrap"
date: 2022-08-06T10:33:40-07:00
categories:
  - Tech
  - Web
tags:
  - Bootstrap
  - HTML
  - CSS
draft: false
---

## Migration From 4 to 5
### Nav
```
// 4
<li class="nav-item {{if eq .PageName `sharing`}}active{{end}}">
  <a class="nav-link" href="sharing-{{.Lang}}.html">

// 5
// Add the .active class on .nav-link to indicate the current page.
<li class="nav-item">
  <a class="nav-link {{if eq .PageName `sharing`}}active{{end}}" href="sharing-{{.Lang}}.html">
```
### Badge
```
// 4
<span class="badge badge-danger badge-pill">2</span>

// 5

```
### Nav
```
// 4
<button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#menu1">

// 5 - data-xxx to data-bs-xxx
<button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#menu1">
```

### Video
```
// 4
<div class="embed-responsive embed-responsive-16by9">
    <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/fZlgYLAtMFs" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>
            
// 5
 <div class="ratio ratio-16x9">
    <iframe src="https://www.youtube.com/embed/fZlgYLAtMFs" allowfullscreen></iframe>
</div>
```

### Dark Mode
```
// 4
<html lang="en">
...
<nav class="navbar fixed-top navbar-expand-lg navbar-dark">

// 5
<html lang="en" data-bs-theme="dark">
...

<nav class="navbar fixed-top navbar-expand-lg" data-bs-theme="dark">

```

### CSS
```
// 4
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css" integrity="sha384-xOolHFLEh07PJGoPkLv1IbcEPTNtaed2xpHsD9ESMhqIYd0nLMwNLD69Npy4HI+N" crossorigin="anonymous">

// 5
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
```
### JS
```
// 4
<script src="https://cdn.jsdelivr.net/npm/jquery@3.5.1/dist/jquery.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js" integrity="sha384-9/reFTGAW83EW2RDu2S0VKaIzap3H66lZH81PoYlFhbGU+6BZp6G7niu735Sk7lN" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/js/bootstrap.min.js" integrity="sha384-+sLIOodYLS7CIrQpBjl+C7nPvqq+FbNUBDunl/OZv93DB7Ln/533i8e/mZXLi/P+" crossorigin="anonymous"></script>

// 5
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
```

## Examples
### Center Alignment
* `text-center`: center the text inside the container row "r1"
* `justify-content-center`: make all three columns "c1", "c2", and "c3" center as a group inside row "r2"; or make the button horizontal center inside column "r2"
* `d-flex align-items-center`: make the button vertical center inside of the "c2" container; you can use `align-items-start` to move the button to the top of "c2" container
* `align-self-center`: make the "c2" container as a whole to be center vertically inside column "r2" ; 

```
<div class="container">
    <div id="r1" class="row">
        <div class="col">
            <h3 class="text-center">Check Threshold Remaining</h3>
        </div>
    </div>
    <div id="r2" class="row justify-content-center" style="min-height: 60vh;">
        <div id="c1" class="col-4 col-md-3 d-flex justify-content-end">
            <textarea name="asset-list" id="asset-list" style="min-width: 100%; overflow: scroll"></textarea>
        </div>
        <div id="c2" class="col-4 col-md-3 align-self-center d-flex align-items-center justify-content-center" style="background-color: lightyellow; height: 30vh">
            <button onclick="getThreshold()">Get Thresholds</button>
        </div>
        <div id="c3" class="col-4 col-md-3">
            <div id="result" style="min-height: 100%; min-width: 100%; overflow: scroll; background-color: aliceblue">
            </div>
        </div>
    </div>
</div>
```

## Text
### Line height
Change the line height with .lh-* utilities.
* lh-1
* lh-sm
* lh-base
* lh-lg
* etc.

```
<p class="lh-1">...</p>
```

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