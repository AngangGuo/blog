---
title: "Notes - Excel"
date: 2020-07-10T13:18:04-07:00
categories:
 - Tech
 - Office
tags:
 - Excel
draft: false
---

## Tips
### How to remove/suppress the `#DIV/0!` error?
Use any of the following formula to remove the #DIV/0! from Spreadsheet:
* `=IFERROR(A1/A2, 0)`
* `=IF(A2,A1/A2,0)` or `=IF(A2,A1/A2,"No Input")`
* `=IF(ISERROR(A1/A2),0,A1/A2)`

## Excel Notes

### How to copy and paste just visible cells only?
#### Keyboard shortcut: 
Select the range, press `ALT + ;` will select only the visible cells.
See [here](https://trumpexcel.com/select-visible-cells/)

#### Sort/Filter area
`Sort & Filter` > `Filter`: You can copy/paste the visible cells of the sorting area. 

#### Select visible cells
* Select the entire range you need to copy.
* Press F5, this opens Go to dialog. Click on Special button.
* Now select visible cells option.
* Now press CTRL + C
* Then go to target sheet and press CTRL + V to paste.

### How to remove duplicated rows?
You can remove duplicate cell values or rows according to your selection.
1. Select the range of cells that has duplicate values you want to remove.
2. Click `Data` > `Remove Duplicates`, and then Under Columns, 
   check or uncheck the columns where you want to remove the duplicates.
 
Note: the duplicate is for the rows you selected, it may contain one or more columns.

### How to highlight duplicate values?
1. Select the cells you want to check for duplicates.
2. Click `Home` > `Conditional Formatting` > `Highlight Cells Rules` > `Duplicate Values`.
3. In the box next to values with, pick the formatting you want to apply to the duplicate values, and then click OK.

Note: This can only highlight individual duplicated cells, not duplicate rows.