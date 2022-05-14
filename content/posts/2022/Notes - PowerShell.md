---
title: "Notes   PowerShell"
date: 2022-05-13T20:45:54-07:00
categories:
  - Tech
  - OS
tags:
  - PowerShell
draft: false
---

## Environment Variables
### How to show environment variables?
* Show all environment variables: `dir env:`
* Show one environment variable: `$Env:METRICSID`

### How to set an environment variable?
* `$Env:METRICSID = "MYID"`
* `$Env:METRICSID += "!"` // MYID!

### How to remove an environment variable?
Set the variable as empty string will remove it.

* `$Env:METRICSID = ""`


