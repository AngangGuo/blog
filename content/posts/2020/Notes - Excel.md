---
title: "Notes - Excel"
date: 2020-07-10T13:18:04-07:00
categories:
  - Tech
  - Application
tags:
  - Excel
  - Office
draft: false
---

## Tips
* To insert current date: `Ctrl+;`
* To add today’s date in such a way that it updates when you recalculate or reopen your spreadsheet: `=Today()`
* To insert current time: `Ctrl+Shift+;`


## Q & A
### How to add Header & Footer ?
It's better to add the file path and name at the bottom of the Excel form so that we can find it easily in the future.

Follow these steps to add header and footer in Excel:
* Go to `Insert` tab, in the `Text` group, click `Header & Footer`
* Excel will display the worksheet in Page Layout view
* To add or edit a header or footer, click the left, center, or right header or footer text box at the top or the bottom of the worksheet page (under Header, or above Footer)
* Type the new header or footer text

To close the header and footer, you must switch from `Page Layout` view to `Normal` view
* On the `View` tab, in the Workbook Views group, click `Normal`.

### How to view file version history?
* Open the file you want to view.
* Click `File` > `Info` > `Version history`. 
* Select a version to open it in a separate window. 
* If you want to restore a previous version you've opened, select Restore.

### How to remove/suppress the `#DIV/0!` error?
Use any of the following formula to remove the #DIV/0! from Spreadsheet:
* `=IFERROR(A1/A2, 0)`
* `=IF(A2,A1/A2,0)` or `=IF(A2,A1/A2,"No Input")`
* `=IF(ISERROR(A1/A2),0,A1/A2)`

### How to paste CSV data into individual columns?
When pasting CSV data into Excel, all the data will be in one column. 
To separate the data into individual columns, follow these steps:

* Data > Text to Columns
* Original data type: Delimited / Fixed width
* (For Delimited)Delimiters: Tab / Semicolon / Comma / Space / Other

Caution: Excel will remember your selection and when you paste data next time, it'll apply these settings automatically.


### How to force Excel to re-calculate all the formulas?
Using keyboard shortcut `Ctrl+Alt+F9` to re-calculate the workbook formulas.

* `F2` – select any cell then press F2 key and hit enter to refresh formulas.
* `F9` – recalculates all sheets in workbooks
* `Shift+F9` – recalculates all formulas in the active sheet
* `Ctrl+Alt+F9` – force calculate open worksheets in all open workbooks including cells that have not been changed
* `Ctrl+Alt+Shift+F9` – recalculates all sheets in all open workbooks

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

### Why the cell can't show out the whole number in Text format?
Excel cell format is `General` by default, therefore it can display up to 11 digits in a cell. 
For numbers more than 11 digits it'll show out as scientific format such as `1.23457E+11`.
At this time even if you apply the `Text` format, the whole number doesn't show out.

You need to set the cell format as `Text` **first**, then enter the long number.

Alternatively, type a single quotation mark (') first in the cell, and then type the long number.
(such as `'1234567890123`)

### How to fill in data into the pre-ordered associate name list?
The `XLOOKUP` function searches a range or an array, and then returns the item corresponding to the first match it finds. 
If no match exists, then XLOOKUP can return the closest (approximate) match. 
```go
=XLOOKUP(lookup_value, lookup_array, return_array,[if_not_found], [match_mode], [search_mode])
```

* The source data(User Metrics) are in column F and G
  * Employee Name in column F from F2 to F24
  * FG(Client) number in column G from G2 to G24
* The pre-ordered associate names are in column B from B2 to B31; The data will be filled into column C (C2:C31)
* Type in the following formula to C2:C31(Example C11): 
  * =XLOOKUP(B11,$F$2:$F$24,$G$2:$G$24,0); This formula means search `Chen, Jun`(B11) in range F2:F24, if found, return data in G2:G24(10); if not found, return 0;

![User Metrics Data](/images/2023/user-metrics-xlookup.PNG)