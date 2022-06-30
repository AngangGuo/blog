---
title: "Notes - UiPath"
date: 2022-06-14T10:36:34-07:00
draft: true
---


## Pitfalls
### Switch Activity
By default, the Switch activity uses the integer argument(int32). 
If using string argument, quotation marks aren't used in the `Case` field. 

For example: use `Case Andrew` instead of `Case "Andrew"`. 
It seems the [doc](https://docs.uipath.com/studio/docs/the-switch-activity) doesn't mention it.

There's options in the Case statement that you can choose (empty) or (null). 
You can also convert the condition into array index, or assign "NA"(or any string reasonable to your situation) to variable if it's empty.

### Use Application/Browser Activity
Option `Open: IfNotOpen`: If the browser is opened, it won't open a new browser instance, nor goes to the URL.

For example: After I run `Login.xaml`, it opens Chrome browser, go to `https://example.com/Login.aspx` and logged in the website. 
The browser is left open in `https://example.com/Welcome.aspx` page. 

If I run this script again without close the first instance, it won't open another browser, nor go to the Login page,
but trys to find the `User Name` at the Welcome page and cause error: `Source: Type Into 'User Name'`

### Try Catch Activity
You need to add `Catch Exception` block to handle the errors(or ignore the error), 
otherwise the script will be terminated because of the un-handled exception.

## Studio
* When duplicates selectors are found, they are highlighted yellow.
* After a Target is indicated, it is recommended to indicate an Anchor for it in order to create a reliable Descriptor.
* You can indicate up to 3 Anchors for any Target.
* The targeting process happens again at runtime, with all three methods searching in parallel for the target. This means that the method that seems to be the fastest at design time might change at runtime.
* If there are both a variable and an argument with the same name, the variable is always defaulted to and used at runtime.
* 

### Modern Experience vs Classic Experience
See [here](https://docs.uipath.com/studio/docs/modern-design-experience#section-differences-between-modern-experience-and-classic-experience)

* Some activities can only be found in Modern Experience, others only in Classic Experience
* You can use Classic Activities in Modern Experience by selecting `Show Classic` from the filter in Activities tab; or use Modern Activities in Classic Experience

### Shortcuts
* Choose variable (Ctrl+Space)
* Choose argument (Ctrl+Shift+Space)
* Create variable (Ctrl+K)
* Create argument (Ctrl+M)

### Verify Execution
At runtime, verifies if the action performed by the activity was correct. 

This is done by indicating an element that should appear or disappear after the action is performed, 
which is monitored and verified after the activity is executed. This feature can be enabled from the Project Settings, 
or from the body of each activity, by selecting Add Verification from the context menu. 

The `Verify Execution` feature is present in the Modern UI Automation experience.

**Possible Usage**
It should be used by an action with a single expected result comes out(UI element appear or disappear, specific text show out, etc). 
Like after I searched the Load ID `L1053099` from the Receiving tab in BlueIQ, the `Pallet Details` should come out, etc.

### Check App State
Checks the state of an application or web browser by verifying if an element appears in or disappears from the user interface.
It can 
* execute one set of activities if the element is found and a different set of activities if the element is not found. 
* monitor an entire application for changes, not only a single UI element. 
* be used as a condition for the `Retry Scope` activity. 
* be used outside a `Use Application/Browser` activity.

### Check state by using loop and condition
For the login condition that `Check App State` can't handle these complex situations, I can use loop and condition check.

For example, after login I want to check if it's success or failed, if failed what kind of problems, etc.

### Descriptor
See [Advanced Descriptor Configuration](https://docs.uipath.com/activities/docs/advanced-descriptor-configuration)

* You can indicate up to 3 Anchors for any Target.
* Three targeting methods: Selector, Fuzzy Selector, and Image.

### DataTable
* In DataTables, individual cells can be identified by using the Column name or zero-based index and the row index.
* The most common activities and methods to create DataTables are:
  * The Build Data Table Activity
  * The Read Range Activities
  * The Read CSV Activity
  * The Data Scraping Action
  * The Generate Data Table From Text Activity
  

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

### Global Exception Handler
* Only one Global Exception Handler can be set per automation project
* `errorInfo` with the `In` direction - contains the information about the error that was thrown and the workflow that failed.
* `result` with the `Out` direction - used for determining the next behavior of the process when it encounters the error.

Next behavior can be:
* Continue - The exception is re-thrown.
* Ignore - The exception is ignored, and the execution continues from the next activity.
* Retry - The activity which threw the exception is retried.
* Abort - The execution stops after running the current handler.

```
errorInfo.RetryCount < 3
result = ErrorAction.Retry // Ignore, Continue, or Abort
```

### Git Integration
* Go to [Github App - UiPath](https://github.com/apps/uipath) > Select the repositories > Install
* 

## Useful VB Functions


## StudioX
StudioX projects are designed for attended use only and we do not recommend using StudioX when developing projects intended for unattended use.

## Troubleshooting

### Why UiPath extension can't be installed in Chrome incognito mode?
The [UiPath extension](https://docs.uipath.com/studio/docs/extension-for-chrome) is installed but can't be used in incognito mode.
See [Enable access to file URLs and incognito mode](https://docs.uipath.com/studio/docs/chrome-extension#enable-access-to-file-urls-and-incognito-mode)
on how to enable it.

### Microsoft Edge - Can't install extension
See [UiPath - Edge Group Policies](https://docs.uipath.com/studio/docs/edge-group-policies)
 and [Microsoft Edge Policies](https://docs.microsoft.com/en-us/deployedge/microsoft-edge-policies#extensioninstallallowlist)

