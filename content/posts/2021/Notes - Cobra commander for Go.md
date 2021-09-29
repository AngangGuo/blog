---
title: "Notes - Cobra Commander for Go"
date: 2021-09-28T11:47:50-07:00
categories:
  - Tech
tags:
  - Go
  - Cobra
draft: false
---

## Creating a CLI program using Cobra Generator
This is a step by step on how to create a Go program to generate a concession report by using Cobra Generator.

* First, get the Cobra Generator
```go
go get github.com/spf13/cobra/cobra
```
* Using the Cobra Generator to create the structure for our program
```go
mkdir -p rl && cd rl
cobra init --pkg-name rl
```

* Create a `report` subcommand
```go
cobra add report
```

Note: 
Although `rl concession` and `rl liquidation` are concise, 
I prefer to use `rl report concession` or `rl report liquidation` for these commands convey information more accurately. 

* Create a `concession` subcommand for `report` so that you can use `rl report concession` to generate the concession report.
```go
// Note: using the variable name of parent command for the sub-command(reportCmd in this case)
// You can find the report command variable name in the rl/cmd/report.go
cobra add concession -p reportCmd
```

* Create a `concession` sub-folder in `rl` and doing the report generating job in `rl/concession/main.go`
```go
// 1. in it's own package(not package main)
package concession
import ...

// 2. export Main
func Main(){...}
...
```

* Update Cobra command file `rl/cmd/concession.go`
```go
package cmd

import (
    // 1. import the concession package
    "github.com/AngangGuo/rl/concession"
    "github.com/spf13/cobra"
)

// concessionCmd represents the concession command
var concessionCmd = &cobra.Command{
    Use:   "concession",
    Short: "Create the Weekly Concession Report",
    Long: `Create the Weekly Concession Report from "Concession v2.xlsx" file. `,
	
    // 2. hook the concession function here
    Run: func(cmd *cobra.Command, args []string) {
        concession.Main()
    },
}

func init() {
    // Cobra added these automatically when running command `cobra add concession -p reportCmd`
    reportCmd.AddCommand(concessionCmd)
}	
```


