---
title: "Notes - UiPath"
date: 2022-06-14T10:36:34-07:00
categories:
  - Programming
  - Web
tags:
  - UiPath
  - Work
draft: false
---

## Work
### How to update RLPath(UiPath project)?
```
// this works on the laptop
git stash
git pull

// If the above command doesn't work well, 
// try to re-clone to a new folder
git clone --depth 1 https://github.com/AngangGuo/RLPath.git RLPath
```

## Control Flow

### How to comment / un-comment an activity?
Select the activity
* To comment an activity: Ctrl + D
* To un-comment an activity: Ctrl + E

### How to verify if `Get Text` is empty?
* Check App State (Element to appear / Element to disappear)
* Retry Scope & Check True(or Check False)
* Do While
* Element Exists / On Element Appear (Classic)

### What's the difference between `Retry Scope` activity and `Do While` condition?
You can ensure the text is gotten by using `Retry Scope` activity or `Do While` condition.
But there're some important difference between them:
* Condition: Use `Check False` activity as condition for `Retry Scope`; Use comparison operator in `Do While` loop
* Error: `Retry Scope` throws error if exceeds the `NumberOfRetries`; `Do While` will retry forever if you don't stop it. 
You can decide if you want to throw error after certain retries or not throw any error. 

### How to select a value from dropdown?
1. For single value: `Select Item` activity > Type in the text of the dropdown you want to select
2. Multiple values: `Select Multiple Items` > List the items you want to select like {"A", "D"}. The type is `String[]`
3. Modern style select box: `Click` activity > `Type Info` activity > `Send Hotkey` (enter)
4. `Find Children` activity(AllItems - IEnumerable<UiElement>) > 
   * `For Each` activity > item (type: UiPath.Core.UiElement)
   * `Get Text` activity: Input Element: item / TheValue)
   * (Alternative)`Get Attribute` activity: item / "aaname" / TheValue
   * `Log Message` > TheValue


### How to use `Invoke code` activity?
Step by step example:
* Build Data Table > Column Name:Costs, Values: 100, 200, 300 > Output: DTCost
* Invoke Code > Edit Arguments: 
  * MyDT / In / DataTable / DTCost
  * MySum / Out / Int32 / Sum
* Invoke Code > Edit Code:
```
Dim row As DataRow
MySum = 0
For Each row In MyDT.Rows
	MySum = CInt(row("Costs").toString) + MySum
Next row
```
* Message Box > Sum.ToString

Note: 
* Arguments `MyDT` and `MySum` are used inside `Invoke Code` activity;
* `DTCost` is DataTable variable created by `Build Data Table` activity
* `Sum` is used to export the value from `Invoke Code`

## Error
### Try Catch activity
* Ctrl + T: Places the selected activity inside the Try section of a Try Catch activity
* If an activity is included in Try Catch, in the Try section, and the value of the ContinueOnError property is True, no error is caught when the project is executed
* Try Catch activity does not catch fatal exceptions
* There is no limit to how many Catches you can use in a Try Catch activity
* This activity requires at least two of the three fields to be in use

## Recommendation
* using PascalCase for variable names. PascalCase is a naming convention in which the first letter of each word in a variable is capitalized. Eg: ItemValue, LastName.
* 

## Pitfalls

### FindChildren Activity (C#)
`FindChildren` will output elements as IEnumerable<UiElement>. I want to locate on of the INPUT box element, the `Contain("INPUT")` method doesn't work in this case.
But `item.GetFriendlyName(false).Trim(' ') == "INPUT"` failed to match it. I checked the string length:
```
msg = item.GetFriendlyName(false)
// msg + ":" +msg.Length
'INPUT  ctl00_ctl00_cont...':28
'INPUT ':8
```
I copied the string and check the ASCII codes from [online tool](https://onlinestringtools.com/convert-string-to-ascii).
I found that the two single quote(apostrophe) `'` are part of the string. To fix it:
```
msg = item.GetFriendlyName(false).Replace("'"," ").Trim(' ')
// msg + ":" +msg.Length
INPUT  ctl00_ctl00_cont...:26
INPUT:5
```

### Input Dialog
Note: The `Result` can be String or other data types like Int32, Double, etc.

* `Options` - An array of options to choose from. If set to contain only one element, a tex box appears to write text. If set to contain 2 or 3 elements, they appear as radio buttons to select from. If set to contain more than 3 items, they appear as a combo box to select from. This field supports only String Array variables. Ex. {"Item1", "Item2", "Item3"}
* `Options String` - A string containing options to chose from. If set to contain only one element, a text box appears to write text. If set to contain 2 or 3 elements, they appear as radio buttons to select from. If set to contain more than 3 items, they appear as a combo box to select from. This field supports only String variables. The option need to be separated with semicolon. Ex. "Item1; Item2; Item3" 
* `Result` - The value(String) inserted by the user in the input dialog. 

Input Dialog will remove all the new line or line feed from the string(but not spaces). For example if I paste the following into the dialog:
```
L956769
L966682 
L979021
```
The result will be `L956769L966682L979021`. If I add two spaces after `L966682`, the result will be `L956769L966682  L979021`.

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

####  How to pass Browser instance between activities?
**Login.xaml**

Use Application/Browser > Properties > 
* Input/Output Element > Output Element > Ctrl+Shift+M > `out_LoginBrowser`(UiElement type)
* Options > Close > Never
* Options > Open > Always

**Main.xaml**

Invoke Workflow File > Workflow file name: Login.xaml > Import Arguments >  assign `out_LoginBrowser` to `io_Browser` argument
Then you can pass the `io_Browser` argument to other activities

**CloseLoadPallets.xaml**

Input Element: in_Browser (pass from io_Browser)

When using Extract Data Table > Extrac Data Wizard > Add Data

If you get error `Target Application could not be identified`
Input Application > Indicate target on screen

**Logout.xaml**
* Input/Output Element > Input Element > Ctrl+M > `in_Browser`(UiElement type)
* Options > Close > Always
* Options > Open > Never


**Main.xaml**

After the browser is closed from Logout.xaml, if you exit the Main.xaml, this error will show out:
```
07/08/2022 11:46:31 RemoteException wrapping System.Exception: Could not retrieve the result of the job execution. 
This might be because a message was too large to process.
```

It is caused by no reference to io_Browser, assign `io_Browser = null` will prevent the error message.

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

## Selectors & Descriptors

### Selectors
* Selectors should be specific and generic at the same time

### Dynamic Selectors
A [dynamic selector](https://docs.uipath.com/studio/docs/dynamic-selectors) uses a variable or an argument as a property for the attribute of your target tag.
```
<webctrl tag='TABLE' />
<webctrl isleaf='1' tableRow='{{RowNumber}}' tag='TD' />
```

Methods:
* The Anchor Base
* The Relative Selector
* The Visual Tree Hierarchy
* The Find Children

See [Advanced Descriptor Configuration](https://docs.uipath.com/activities/docs/advanced-descriptor-configuration)

* You can indicate up to 3 Anchors for any Target.
* Three targeting methods: Selector, Fuzzy Selector, and Image.

## DataTable
* In DataTables, individual cells can be identified by using the Column name or zero-based index and the row index.
* The most common activities and methods to create DataTables are:
  * The Build Data Table Activity
  * The Read Range Activities
  * The Read CSV Activity
  * The Data Scraping Action
  * The Generate Data Table From Text Activity
  
* Build Data Table
* For Each Row
* Add Data Row (row.itemArray, )

### ArrayRow
Some activities like `Add Data Row` can add ArrayRow to a data table. To create an array row:
* Constants: {"Andrew", "Guo"}
* Variable: {FirstName, LastName}

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

## Excel File Processing
UiPath offers two types of design experience (Modern and Classic) each with two separate ways of accessing and manipulating workbooks:

### Workbook - File Access Level: `System > File > Workbook`

* All workbook activities will be executed in the background.
* Doesn't require Microsoft Excel to be installed, 
* The file should not be open in Excel at runtime.
* Works only for .xls and .xlsx files but not .xlsm files
* Workbook activities do not require a scope. The Excel file needs to be indicated in the properties for each individual activity.


### Excel - Excel App Integration: `App Integration > Excel`
(offers different options depending on whether you are using the Modern or Classic design experience).

* Microsoft Excel must be installed
* Works with .xls, .xlsx and .xlsm, and it has some specific activities for working with .csv. 
* All activities can be set to either be visible to the user or run in the background. 
* The file can be open in Excel at runtime.
* If using any activities inside `App Integration > Excel`, these activities must be inside `Excel Application Scope`
* If the same workflow deals with information from two or more Excel files, an Excel Application Scope has to be used for each file.


## Error Handling
* If (Find Children) activity is included in `Try Catch` and the value of the `ContinueOnError` property is True, no error is caught when the project is executed.
* 
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
### Git Integration Prerequisite
GIT integration in Studio requires the [Microsoft Visual C++ 2015 Redistributable Update 3](https://www.microsoft.com/en-us/download/details.aspx?id=53587) version.
Download and install `vc_redist.x64.exe` or `vc_redist.x86.exe` to enable Git integration.

### GitHub Integration
Install UiPath GitHub App to access your repository:
* Go to [Github App - UiPath](https://github.com/apps/uipath) > Select the repositories > Install

## Useful VB Functions
### String
```
Name = "Andrew"
Name.Length // 6
Name.Replace("rew","y") // Andy
Name.Substring(0,1) // A
"3 Years".Split(" "c)(0) // 3

String.Concat (VarName1, VarName2)
VarName.Contains (“text”)
String.Format(“{0} is {1}”, VarName1, VarName2)
VarName1.IndexOf(“a”)
VarName1.LastIndexOf("author")
String.Join(“|”, CollVarName1)
VarName.Replace (“original”, “replaced”)
VarName.Split(“|“c)(index)
VarName1.Substring(startIndex, length)
String.IsNullOrEmpty(DayOfTheWeek)
InputData = InputData.Replace("<day_of_week>",DayOfTheWeek.Trim)

myArr = "A,B,C".Split(","c) // String[] - {"A","B","C"}
String.Join("-", myArr) // A-B-C

InitialMessage = "You searched for author William Shakespeare. His books can be found in the following sotres: Bookland, The Bookshop, Downtown Books, Classics bookstore."
author = InitialMessage.Split("."c).First.ToString.Substring(InitialMessage.LastIndexOf("author")+"author".Length).Trim

FirstName = Row(4).ToString.Substring(0, Row(4).ToString.IndexOf(" "))

String.Format("Availability for {0}: {1}", Author, String.Join(","c+vbcr,Bookstores))

properTemp = StrConv(City, VBStrConv.ProperCase)

```

### Other
```
isStudent = True // False

Age = 20 // int32
"Your " + Age.ToString + "years old."

strYear = "3"
dblYear = CDbl(strYear) // 3.00 - string to double
Math.Round(2.34123, 2) // 2.34

DateTime.ParseExact(VarDate.ToString.Substring(0) ,“dd-MMM-yyyy”,System.Globalization.CultureInfo.InvariantCulture).ToString(“d-MMM-yyyy”)
```

### Collection
#### List
List - System.Collections.Generic.List<T>: used to store multiple values of the same data type, just like Arrays. Unlike Arrays, their size is dynamic.

```
myArr.ToList // List<string>(3){"A","B","C"}
AllCities = Enumerable.Concat(SpainCities.AsEnumberable, UKCilites.AsEnumberalbe).ToList
```

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

### RegEx Builder
Methods in UiPath that use the RegEx builder: Matches, IsMatch, and Replace
* Matches: Searches an input string for all occurrences and returns all the successful matches. Output datatype: System.Collections.Generic.IEnumerable<System.Text.RegularExpressions.Match>
* IsMatch: Indicates whether the specified regular expression finds a match in the specified input string. Output datatype: Boolean
* Replace: Replaces strings that match a regular expression pattern with a specified replacement string. Output datatype: String
 
For Each activity: Keep in mind that the TypeArgument should be Match in System.Text.RegularExpressions.

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
## Useful C# Functions
```
// Input Dialog
new string[] {"1. Close Pallets", "2. QC - Liquidation"}
MyChoise.StartWith("2.")

new System.Exception("Main.xaml: Login failed. ")

// Get the load id from "L956769L966682L979021" or "L956769 L966682  L979021"
LoadIDs.Replace("L", " L").Split(new []{" ",";"},System.StringSplitOptions.RemoveEmptyEntries)

// Split asset tags like: "LPNN798834755LPNRRCI6927711lpnn170702279"
AssetTags.ToUpper().Replace("LPN", " LPN").Split(new []{" "},System.StringSplitOptions.RemoveEmptyEntries)

// Use Environment.NewLine instead of "\n" for the "Input Label" prompt in Input Dialog Activity 
"Please type the Load IDs:"+Environment.NewLine+"Note: You can type in multiple load ids separeted by spaces or new lines."

// Initialize dictionary
Dictionary<string, string> AssetInfo = new Dictionary<string, string> {}
AssetInfo.Add("SourceLocation", "YVR3")
AssetInfo["SourceLocation"] = YVR3

// For Each activity to loop through a dictionary AssetInfo
item type is System.Collections.Generics.KeyValuePair<System.String, System.String>
```

## Misc
### How to create a generic Windows credential?
* Control Panel > User Accounts > Credential Manager > Windows Credentials > Add a generic credential.
* Install `UiPath.Credentials.Activities = 2.0.0` from Manage Packages to use the Windows generic credentials.
* Use a `Get Credentials` activity. In the Properties Panel, create a variable called Username using control + K to store the username value and a variable called Password to store the password value.
* Use `Type Into` activity to enter the email stored in the Username variable and indicate the Username field on the Acme website. Enable the SimulateType option from the Properties Panel so that the action can be performed in the background.
* Use a `Type Secure Text` activity to enter the password stored in the Password variable. Enable the SimulateType option from the Properties Panel.

Note: You can't use `Type Into` activity to input the Password text. `Can't convert secure text to string`

```
Source: Get Credential

Message: Could not find the asset 'https://acme-test.uipath.com/login'. Error code: 1002; Asset name: https://acme-test.uipath.com/login

Exception Type: UiPath.Core.Activities.OrchestratorHttpException
```
You need to create the same credentials in UiPath Orchestrator:

`Login to https://platform.uipath.com/ > Orchestrator > Assets > Add asset > Create a new asset`
* Asset name: https://acme-test.uipath.com/login
* Type: Credential
* Username: myEmail
* Password: myPassword
* Create


## Troubleshooting

### Why UiPath extension can't be installed in Chrome incognito mode?
The [UiPath extension](https://docs.uipath.com/studio/docs/extension-for-chrome) is installed but can't be used in incognito mode.
See [Enable access to file URLs and incognito mode](https://docs.uipath.com/studio/docs/chrome-extension#enable-access-to-file-urls-and-incognito-mode)
on how to enable it.

### Microsoft Edge - Can't install extension
See [UiPath - Edge Group Policies](https://docs.uipath.com/studio/docs/edge-group-policies)
 and [Microsoft Edge Policies](https://docs.microsoft.com/en-us/deployedge/microsoft-edge-policies#extensioninstallallowlist)

### Wrong language in project file
Error message: 
`Expression activity type ‘CSharpValue`1’ requires compilation inorder to run`

If a project missing `project.json` file, UiPath will automatically create one when you open your project.
The default design language is VB(VisualBasic), but my project is C#. 

Change the `expressionLanguage` to `CSharp will solve the problem.

`"expressionLanguage": "CSharp",`



