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

### Tips
* Make sure to save `f.Save()` the Excel file after modify it.

### Common Functions
Open a file
```
wtwFile, err := excelize.OpenFile("wtw.xlsx)
```

Get the sheet list
```
sheetNames := wtwFile.GetSheetList()
```

Create a new sheet
```
err := wtwFile.NewSheet("Master")
```

Delete a sheet
```
// No error returned if the sheet not exist
err := wtwFile.DeleteSheet(masterSheetName)
```
Rename a sheet name
```
err := wtwFile.SetSheetName(tempSheetName, masterSheetName)
```

Set cell value
```
wtwFile.SetCellValue(SheetName, "A1", "Pallet#")
```

set cell value as Int
```
templateFile.SetCellInt(sheetName, currentCell, int64(cat.LT21))
```

Get sheet data
```
tempRows, err := wtwFile.GetRows(tempSheetName)
```

Set formula
```
cogsVolumeFormula := fmt.Sprintf("=%s/%s", currentCell, overallCOGSCell)
currentCell, _ = getCellNameInColumn(currentCell, 1)
_ = templateFile.SetCellFormula(sheetName, currentCell, cogsVolumeFormula)
```
Note: See below on how to re-calculate the formula when opening the file in Excel.

Save Excel file
```
err := wtwFile.Save()
```
### How to set cell value in a row sequentially?
"A4": fmt.Sprintf("%s4", string(rune(65)))

```
    asciiA := 65
	titles := []string{"Pallet#", "Load ID", "Type", "Sorter", "LPNs", "Location"}
	for i, title := range titles {
		// Title on row#1: A1: Pallet#, B1: Load ID, etc.
		err := wtwFile.SetCellValue(tempSheetName, fmt.Sprintf("%s1", string(rune(asciiA+i))), title)
		if err != nil {
			return err
		}
	}
```

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

### Force Excel to Recalculate on Open (Writing Files)
When writing data into a spreadsheet by using Go Excelize, 
the formulas in the sheet doesn't update automatically.

You can update the formula cells manually from Excel by pressing `Ctrl+Alt+Shift+F9` or `Ctrl+Alt+F9`. 

If you are writing data to cells and want the formulas to update automatically 
when a user opens the file in Microsoft Excel, use the `UpdateLinkedValue()` method BEFORE saving. 
This clears cached values inside the spreadsheet, 
triggering Excel's native calculation engine to run immediately upon opening.
```
// Write data to cells in the sheet that contains formulas.
if err := f.UpdateLinkedValue(); err != nil {
    log.Fatal(err)
}

if err := f.SaveAs("Book1.xlsx"); err != nil {
    log.Fatal(err)
}
```

### Compute a Cell Value Instantly (Reading Files)
If your application needs to fetch the calculated result of a formula directly 
inside your Go code without opening Excel, use `CalcCellValue()`.

Limitation: 
This only evaluates the specific cell you target and 
supports a subset of standard Excel functions (like SUM, AVERAGE, VLOOKUP). 
It does not support complex layout mechanics like iterative calculations or array formulas.
```
// Evaluate and fetch the real-time calculated string value of the cell
result, err := f.CalcCellValue("Sheet1", "A3")
if err != nil {
    log.Fatal(err)
}
```