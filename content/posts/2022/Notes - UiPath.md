---
title: "Notes - UiPath"
date: 2022-06-14T10:36:34-07:00
draft: true
---

## Recommendation
* using PascalCase for variable names. PascalCase is a naming convention in which the first letter of each word in a variable is capitalized. Eg: ItemValue, LastName.
* 
## Pitfalls
### Input Dialog
Note: The `Result` can be String or other data types like Int32, Double, etc.

* `Options` - An array of options to choose from. If set to contain only one element, a tex box appears to write text. If set to contain 2 or 3 elements, they appear as radio buttons to select from. If set to contain more than 3 items, they appear as a combo box to select from. This field supports only String Array variables. Ex. {"Item1", "Item2", "Item3", "Item4", "Item5"}
* `Options String` - A string containing options to chose from. If set to contain only one element, a text box appears to write text. If set to contain 2 or 3 elements, they appear as radio buttons to select from. If set to contain more than 3 items, they appear as a combo box to select from. This field supports only String variables.
* `Result` - The value inserted by the user in the input dialog. 

### (Data Table)Add Data Row
Inside `For Each Row` activity, the myRow in `ForEach myRow in dt_Data1` can't be used directly in `Add Data Row` activity:
```
This row alread belongs to another table
```
Use `myRow.itemArray` instead

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

### Arguments
* There're three types of arguments: IN, OUT, IN/OUT; Use `Property` if the argument is not used but you want to keep it.
* Argument names should be in PascalCase with a prefix stating the argument direction, such as in_DefaultTimeout, in_FileName, out_TextResult, io_RetryNumber.
* Do not require a scope
* Can be created even if the Designer panel doesn't contain any activity
* Used to pass data between automation

#### How to pass data by using argument?
* Invoke the Workflow File
* Click the `Import Arguments` button
* Assign the out arguments from child workflow to variables in parent process.
* Or from parent workflow assign data to in arguments of the child for it to be used in child process.

### Shortcuts
* Choose variable (Ctrl+Space)
* Choose argument (Ctrl+Shift+Space)
* Create variable (Ctrl+K)
* Ctrl+M: Create In argument
* Ctrl+Shift+M: Create Out argument
* Ctrl+F5: Run project
* 
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
  
* Build Data Table
* For Each Row
* Add Data Row (row.itemArray)

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

### Processing Excel File
* If using any activities inside `App Integration > Excel`, these activities must be inside `Excel Application Scope`
* For any activities inside `System > File > Workbook`, they can be used directly

## Error Handling
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

### Useful Links
* [Creating Error-Proof Resuable Workflows In UiPath](https://forum.uipath.com/t/creating-error-proof-reusable-workflows-in-uipath/331675)

## Version Control
### Git Integration
* Go to [Github App - UiPath](https://github.com/apps/uipath) > Select the repositories > Install
* 

## Useful VB Functions
### String
```
Name = "Andrew"
Name.Length // 6
Name.Replace("rew","y") // Andy
Name.Substring(0,1) // A
"3 Years".Split(" "c)(0) // 3

myArr = "A,B,C".Split(","c) // String[] - {"A","B","C"}
String.Join("-", myArr) // A-B-C

myArr.ToList // List<string>(3){"A","B","C"}

InitialMessage = "You searched for author William Shakespeare. His books can be found in the following sotres: Bookland, The Bookshop, Downtown Books, Classics bookstore."
author = InitialMessage.Split("."c).First.ToString.Substring(InitialMessage.LastIndexOf("author")+"author".Length).Trim

isStudent = True // False

Age = 20 // int32
"Your " + Age.ToString + "years old."

strYear = "3"
dblYear = CDbl(strYear) // 3.00 - string to double
Math.Round(2.34123, 2) // 2.34
```

### Collection
#### List
List - System.Collections.Generic.List<T>: used to store multiple values of the same data type, just like Arrays. Unlike Arrays, their size is dynamic.

#### Dictionary
Dictionary - System.Collections.Generic.Dictionary<TKey, TValue>: used to store objects in the form of (key, value) pairs, where each of the two can be of a separate data type.

#### Array
**Array** - `ArrayOf<T>` or `System.DataType[]`: used to store multiple values of the same data type. The size (number of objects) is defined at creation.

Note: As a good case practice, arrays are used for defined sets of data (for example, the months of the year or a predefined list of files in a folder). 
Whenever the collection might require size changes, a List is probably the better option.

From Variables panel > Variable type > Array of [T] > Select Types > String > []string
```
StrArray = {"Monday", "Tuesday", "Wednesday"}
// StrArray(0) -> "Monday"
// You can change array value, but can't change array size
StrArray(1) = "Sunday"
String.Join(" ",StrArray) // Sunday Tuesday Wednesday

```

### Date and Time
* DateTime - System.DateTime: Used to store specific time coordinates (mm/dd/yyyy hh:mm:ss).
* TimeSpan - System.TimeSpan: Used to store information about a duration(dd:hh:mm:ss).
* 
```
DateTime.Now

```

### GenericValue
The GenericValue (UiPath.Core.GenericValue) data type is particular to UiPath and can store any kind of data, including text, numbers, dates, and arrays. 
This type is mainly used in activities in which we aren't sure what type of data we'll receive, yet in general, using this is temporary.

UiPath Studio has an automatic conversion mechanism of GenericValue variables and arguments, which you can guide towards the desired outcome by carefully defining their expressions.

Please remember that GenericValue variables are a temporary solution at most. When it becomes clear what the data type should be, we strongly recommend that you change it to the specific type.

When "+" is used between Generic Value variables, it is interpreted based on the value of the first variable.
* The "+" operator is interpreted as Concatenation in the case of Strings.
* The "+" operator is interpreted as an Addition in the case of Int32.

### Exception
```
// create exception
New BussinessRuleException(“Password is too short”)
```

### Convert
```
StrVar = Convert.Tostring(IntVar)
StrVar = DblVar.ToString
StrVar = BoolVar.ToString()
StrVar = Now.ToString(“dd-MM-yyyy”) // DateTimeVar.ToString("dd-MM-yyyy")

IntVar = ToInt32(StrVar)
IntVar = CInt(StrVar)
IntVar = ToInt32(DblVar) 
DblVar = Parse(StrVar) 
BoolVar = ToBoolean(IntVar)
```
### Useful C# Functions
```
new System.Exception("Main.xaml: Login failed. ")
```

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

