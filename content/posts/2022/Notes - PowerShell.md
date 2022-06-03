---
title: "Notes - PowerShell"
date: 2022-05-13T20:45:54-07:00
categories:
  - Tech
  - OS
tags:
  - PowerShell
draft: false
---

## Tips
### How to execute one command after another?
```
go build -o server.exe; .\server.exe
```
To execute the second command only if the first command is succeed:
```
go build; if ($?) {.\server.exe}
```

## Environment Variables
### How to show environment variables?
* Show all environment variables: `dir env:`
* Show one environment variable: `$Env:METRICSID`

### How to set an environment variable?
* `$Env:METRICSID = "MYID"`
* `$Env:METRICSID += "!"` // MYID!
* `$Env:PATH += ";C:\bin"`

### How to save environment variable into a text file?
```
$Env:PATH >> path.txt
```

### How to remove an environment variable?
Set the variable as empty string will remove it.

* `$Env:METRICSID = ""`


