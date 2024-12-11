---
title: "Notes - Power Automate"
date: 2024-11-21T12:40:06-08:00
categories:
  - Programming
  - RPA
tags:
  - Power Automate
  - Work
draft: false
---

## Tips
### Variable inside a message
* You can use a variable on its own: `=ExcelData`
* Or you can add the variable inside a message: `The data is: ${ExcelData}.`

### Loop number
* Start from: `=1`
* End to: `=3`
* Increment by: `=1`

If you type `1` in the `Start from` field, it will be treated as a string and cause error:
```
Expression must contain a 'Numeric value'
```

### Excel row & column number
* Action: `Read from Excel worksheet`
  * Start Column: `A`
  * Start row: `1`

### fx 
* Equal: `=` (If '=ExcelData = ""' then)
* Calculate: `=LoopIndex + 1`
* 
