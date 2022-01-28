---
title: "Tips - Goland"
date: 2020-04-27T16:34:31-07:00
categories:
 - Tech
 - Tools
tags:
 - Goland
draft: false
---


## Tips to speed up editing
### Keyboard Shortcuts
```
Ctrl + Alt + Left: Back to previous caret position
Ctrl + Alt + Right: Forward to next caret position

Ctrl + .: fold / unfold selection
Ctrl + -: fold 
Ctrl + =: unfold

Ctrl + Alt + Shift + J: Select all occurrences of a word
Alt + Enter: Fill in struct fields
Ctrl + B: Go to declaration usage
```

### Customize Keymap
Sort Lines:
```
Alt + Shift + L: Sort lines (select the lines and hit this key. I use it to sort names)
```

### Group declarations
Goland will expand `vars` -> `Tab` to group declaration:
```
var (
    name =
)
```
Similar for `consts`, `types` and `imports`

### `For` loop
Type `fori` -> `tab` will expand to:
```
for i := 0; i < ; i++ {
	
}
```

### View Tips
#### Soft Wrap
From `Settings > Editor > General > Soft Wraps`, You can config soft wraps for specific file types. Enter file extensions separating them with semicolon.

You can also enable or disable soft wraps right in the editor:
Right-click the left gutter and from the context menu, either select or clear the Soft-Wrap Current Editor option. 
Keep in mind that these settings affect only the current editor, not a file. 

### Select text vertically?
#### Option 1: Drag the mouse
* Click the mouse wheel and drag; Or
* Hold `Alt + Shift` keys and left click the mouse and drag

#### Option 2: In `Column Selection Mode`
Press `Alt + Shift + Insert`, or go to `Edit` > click `Column Selection Mode`,
* Mouse: click the left button and drag the mouse on the text area you want to select.
* Keyboard: use `Shift + Arrows` to select a block

### How to fix grammar error?
To fix a spelling or grammar error, press `Alt+Enter` to see the list of available actions.

### How to run `Goland` as a lightweight editor from command line?
You can open a file directly from command line like notepad editor without creating a project first.

To open a file or folder from command line, type
```
goland.bat [--line <number>] [--column <number>] <path ...>
# To open a single file
goland --line 5 main.go
# To open a folder in Goland
goland project1
```

Or you can compare two files from command line
```
goland.bat diff <path1> <path2>
```

#### Add `goland.bat` to Windows `PATH`
Add the `C:\Program Files\JetBrains\GoLand 2020.1.1\bin` into Windows `PATH` environment variable  or 
create a batch file `goland.bat` with these lines in a folder that is in your system `PATH`:
```
@echo off
"C:\Program Files\JetBrains\GoLand 2020.1.1\bin\goland.bat" %*
```

## Testing
See [Test Driven Development with GoLand](https://blog.jetbrains.com/go/2020/03/13/test-driven-development-with-goland/)

### Generate tests
#### From testing file
Navigating to your test file(eg. hello_test.go) press `Alt + Insert` to show `generate feature` window.
You can chose to generate Test / Table Test / Benchmark ... from the window.

#### From source file
Navigating to your source file, press `Alt + Insert` to show `Generate` window.
Select `Tests for file` to generate the tests for the source file.

### Run the test
Press `Ctrl + Shift + F10`  (Run context configuration) to run the test.

### Create Function
When testing failed, if it's undefined function error, highlight the function name in test file, 
press `F2` then `Alt + Shift + Enter` to create the function.

### Auto Test
To run the tests automatically on changes, select `Toggle auto-test` from Run tab and adjust the delay parameters.

![auto_test](/images/2020/goland_auto_test.jpg) 

### Refactor
To refactor a function, press `Ctrl + Alt + Shift + T` to show the `Refactor this` window.
Chose the function from the window:
* Rename: `Shift + F6`
* Change Signature: `Ctrl + F6`
* Move: `F6`
* Copy File: `F5`

## Troubleshooting

### Unresolved Reference Error
After creating a new project, I get the echo library(`go get github.com/labstack/echo/v4`), using it in main file, 
but GoLand can't find any methods and highlight them in red:
`Unresolved Reference xxx`

Try these solutions: 
1. File > Invalidate Caches... > Select Clear file system cache and Local History > Ok > Restart GoLand
1. File > Settings > Go > GOPATH: Index entire GOPATH > Apply
1. Or change the module name in `go.mod`
