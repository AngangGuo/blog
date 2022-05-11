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

## Go Excelize
[Excelize](https://github.com/qax-os/excelize) is a library written in pure Go providing a set of functions that allow 
you to write to and read from XLAM / XLSM / XLSX / XLTM / XLTX files.

### How to get the raw data from a cell?
Cell `E1` holds date `1/5/2022` with style as `1/5`

```
// Normal function
cellE, _ := f.GetCellValue(config.ClientSheetName, "E1") 
// Value: 1/5; Type: string
fmt.Printf("Value: %v; Type: %T\n", cellE, cellE)

// Get raw value
cell, _ := f.GetCellValue(config.ClientSheetName, "E1", excelize.Options{RawCellValue: true})
// Value: 44566; Type: string
fmt.Printf("Value: %v; Type: %T\n", cell, cell)

// Value: 44566; Type: float64
excelDate, _ := strconv.ParseFloat(cell, 64)

// Value: 2022-01-05 00:00:00 +0000 UTC; Type: time.Time
excelTime, err := excelize.ExcelDateToTime(excelDate, false)

// Value: 1/5; Type: string
excelTime.Format("1/2")
```

## Tips
* Make sure to save `f.Save()` the Excel file after modify it.

## FAQ

### Why the formula cell doesn't updated(re-calculate)?
When writing data into a spreadsheet by using Go Excelize, the formulas in the sheet doesn't update automatically.

You can't update the Excel formula cells by using Excelize. See [#757](https://github.com/qax-os/excelize/issues/757)
But you can update the formula cells manually from Excel by pressing `Ctrl+Alt+Shift+F9` or `Ctrl+Alt+F9`. 

