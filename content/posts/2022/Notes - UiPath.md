---
title: "Notes - UiPath"
date: 2022-06-14T10:36:34-07:00
draft: true
---

## Studio

### DataTable
#### `Extract Table Data` Activity
Extract tabular data from a specific web page(table) or application(Excel)



#### `Select()` Method
Datatable.Select() is a part of .net framework and returns an array of DataRow objects. 
To convert DataRow() to DataTable, you have to use CopyToDataTable.
```
dtFilter = dtOrder.Select("[Facility]='YVR3' OR [Facility]='YYC1' AND [Threshold]<16").CopyToDataTable

// drFilter type is DataRow; dtFilter type is DataTable
drFilter = dtTablaTotal.Select(“FechaProd >20/02/2020 00:00:00”)
dtFilter = drFilter.CopyToDataTable
```

### Git Integration
* Go to [Github App - UiPath](https://github.com/apps/uipath) > Select the repositories > Install
* 

## StudioX
StudioX projects are designed for attended use only and we do not recommend using StudioX when developing projects intended for unattended use.

## Troubleshooting

### Microsoft Edge - Can't install extension
See [UiPath - Edge Group Policies](https://docs.uipath.com/studio/docs/edge-group-policies)
 and [Microsoft Edge Policies](https://docs.microsoft.com/en-us/deployedge/microsoft-edge-policies#extensioninstallallowlist)

