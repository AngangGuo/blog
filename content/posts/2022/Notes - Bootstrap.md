---
title: "Notes   Bootstrap"
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

## Background
### Background Color
```
<div class="row my-5" style="background-color:rgba(241,216,142,0.6);">
```

### Background Image
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

