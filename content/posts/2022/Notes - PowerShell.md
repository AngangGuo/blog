---
title: "Notes - PowerShell & CMD"
date: 2022-05-13T20:45:54-07:00
categories:
  - Tech
tags:
  - PowerShell
  - CMD
  - Command Line
draft: false
---

## Tips

### How to open terminal in the current folder?
1. Right-click the folder > Open in Terminal
2. From Windows Explorer address bar > `wt -d .`

### Execute multiple commands on one line
#### Windows CMD: `go build | try`
    * &: separate multiple commands on one command line.
    * &&: run the command following && only if the command preceding the symbol is successful. `mkdir -p newApp && cd newApp`
    * ||: run the command following || only if the command preceding || fails.

#### For Powershell or Bash(semicolon): 
New version:  
  * `go build && .\try.exe`: Execute the second command only if the first one succeeds
  * `go build || .\try.exe`: Execute the second command only if the first one fails

Old version:
  * `go build; .\try.exe`: Execute the first command, then second command
  * `go build; if ($?) {.\server.exe}`: To execute the second command only if the first command is succeed

### How to find the location of an executable file?
From command line:
```
G:\>where hugo
C:\Andrew\prg\bin\hugo.bat
```

From PowerShell:
```
PS C:\Andrew\prj\blog> Get-Command hugo
CommandType     Name       Version    Source
-----------     ----       -------    ------
Application     hugo.bat   0.0.0.0    C:\Andrew\prg\bin\hugo.bat
```

### How to change character encoding?
Beginning in PowerShell 5.1, the redirection operators (> and >>) call the Out-File cmdlet,
which is UTF-16 encoding by default. To change the encoding to UTF-8:
```
$PSDefaultParameterValues['Out-File:Encoding'] = 'utf8'
.\plan-b.exe | Out-File plan.txt -encoding utf8
```

### How to get PowerShell version
```
> $PSVersionTable
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

### How to display environment variables?
```
// dir and gci are both aliases for Get-ChildItem
// dir env:
PS > gci env: | where name -like 'METRICS*'
Name                           Value
----                           -----
METRICSID                      CORPORATE\mynameid
METRICSPASSWORD                MYPASSWORD
```

