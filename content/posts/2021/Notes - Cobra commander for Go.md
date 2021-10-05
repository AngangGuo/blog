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

## Creating a CLI program by using Cobra Generator
This is a step by step on how to create a Go program to generate a liquidation report by using Cobra Generator.

* First, get the Cobra Generator
```go
go get github.com/spf13/cobra/cobra
```
* Using the Cobra Generator to create the structure for our program
```go
mkdir -p rl && cd rl
cobra init --pkg-name rl
```

### How to add sub-commands?
* Create a `report` subcommand
```go
cobra add report
```

Note: 
Although `rl liquidation` are concise, 
I prefer to use `rl report liquidation` for this convey information more accurately. 
Later on I can have `rl report concession`, `rl report sn`, etc.

* Create a `liquidation` subcommand for `report` so that you can use `rl report liquidation` to generate the liquidation report.
```go
// -p, --parent string   variable name of parent command for this command (default "rootCmd")
// Note: using the variable name of parent command(reportCmd in this case)
// You can find the report command variable name in the rl/cmd/report.go
cobra add liquidation -p reportCmd
```

* Create a `liquidation` sub-folder in `rl` and doing the report generating job in `rl/liquidation/main.go`
```go
// 1. in it's own package(not package main)
package liquidation
import ...

// 2. export Main
func Main(){...}
...
```

* Update Cobra command file `rl/cmd/liquidation.go`
```go
package cmd

import (
    // 1. import the liquidation package
    "github.com/AngangGuo/rl/liquidation"
    "github.com/spf13/cobra"
)

// liquidationCmd represents the liquidation command
var liquidationCmd = &cobra.Command{
    Use:   "liquidation",
    Short: "Create the Weekly RL Liquidation Report",
    Long: `Create the Weekly RL Liquidation Report from Inventory All Fields file.

1. Download the latest Inventory All Filelds CSV file from Egnyte. 
2. Create the Weekly RL Liquidation Report from this CSV file.

Usage:
rl report liquidation: Create Weekly RL Liquidation Report

Options:
rl report liquidation --download=false (-d=false): Skip downloading Inventory All Fields CSV file
rl report liquidation --amazon (-a): RL Liquidation Report and Amazon Liquidation Report     
     
`,
	
    // 2. hook the liquidation function here
    Run: func(cmd *cobra.Command, args []string) {
        liquidation.Main()
    },
}

func init() {
    // Cobra added these automatically when running command `cobra add liquidation -p reportCmd`
    reportCmd.AddCommand(liquidationCmd)
}	
```

* Add `rl report liquidation` and other commands in the same way as above

### How to add local flags?
* Add flags: to add `download` and `amazon` flags in `cmd/liquidation.go`:
```go
func init() {
	reportCmd.AddCommand(liquidationCmd)

    // 1. Add local flags to liquidation subcommand
	liquidationCmd.Flags().BoolP("download", "d", true, "Download Inventory All Fields CSV file")
	liquidationCmd.Flags().BoolP("amazon", "a", false, "Create Weekly Amazon Liquidation Report")
}
```

* Get flag value: 
```go
...
	Run: func(cmd *cobra.Command, args []string) {
	    // 2. Get local flags' value
		download,err:=cmd.Flags().GetBool("download")
		if err != nil {
			log.Fatalln(err)
		}
		amazon,err:=cmd.Flags().GetBool("amazon")
		if err != nil {
			log.Fatalln(err)
		}
		
		option := liquidation.Option{
			Download: download,
			Amazon:   amazon,
		}
		
		liquidation.Main(option)
	},
}
```

## Links
* https://pkg.go.dev/flag
* https://github.com/spf13/cobra
* https://github.com/spf13/viper