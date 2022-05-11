---
title: "Notes - Go Excelize Usage"
date: 2022-05-10T16:53:39-07:00
categories:
  - Tech
  - Programming
tags:
  - Go
  - Excel
draft: false
---

## Tips
* Make sure to save `f.Save()` the Excel file after modify it.

## FAQ

### Why the formula cell doesn't update(re-calculate)?
When writing data into spreadsheet by using Go Excelize, the formulas in the sheet doesn't update automatically.
You can't update the Excel formula cells by using Excelize. See [#757](https://github.com/qax-os/excelize/issues/757)
But you can update the formula cells manually by pressing `Ctrl+Alt+Shift+F9` from Excel. 

