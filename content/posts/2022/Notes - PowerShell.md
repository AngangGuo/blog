---
title: "Notes - PowerShell & CMD"
date: 2022-05-13T20:45:54-07:00
categories:
  - Tech
  - Command Line
tags:
  - PowerShell
  - CMD
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
Show all environment variables: 
* PowerShell: `dir env:`
* CMD: `SET`

Show one environment variable: 
* PowerShell: `$Env:METRICSID`
* CMD: `SET METRICSID`

### How to set an environment variable?
PowerShell:
* `$Env:METRICSID = "MYID"`
* `$Env:METRICSID += "!"` // MYID!
* `$Env:PATH += ";C:\bin"`

CMD:
* `SET METRICSID="MYID"` // Only this session
* `SETX METRICSID MYID` // Global

### How to save environment variable into a text file?
```
$Env:PATH >> path.txt
```

### How to remove an environment variable?
Set the variable as empty string will remove it.

* `$Env:METRICSID = ""`


