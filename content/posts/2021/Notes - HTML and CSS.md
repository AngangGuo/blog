---
title: "Notes   HTML and CSS"
date: 2021-03-11T14:10:32-08:00
draft: true
---

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