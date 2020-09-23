---
title: "Notes   Go Programming Language"
date: 2020-07-31T14:47:26-07:00
draft: true
---

## Other
### How to get the type of a variable?
```
v := []string{"a", "b"}
// using %T
fmt.Printf("%T", v) // []string
// using reflect package
fmt.Println(reflect.TypeOf(v)) // []string
fmt.Println(reflect.ValueOf(v).Kind()) // slice
```

### How to cross compile Go program?
To cross-build executables for other Go-supported platform,
just set `GOARCH` and `GOOS` to the target platform and build.
```
// From Windows
set GOARCH=amd64
set GOOS=linux
go build

// From Linux
$ env GOOS=windows GOARCH=amd64 go build
```

## Time
### time.Add() vs time.AddDate()
```
    days := 2
	t := time.Now()
	// 2009-11-10 23:00:00 +0000 UTC m=+0.000000001

    // Add mostly is used to process less than 24 hours
	t1 := t.Add(time.Hour * 24 * time.Duration(days)).Format("2006-01-02")
	fmt.Println(t1) // 2009-11-12

    // AddDate is easier when working with days, months, or even years, etc.
	t2:=t.AddDate(0,0,2).Format("2006-01-02")
	fmt.Println(t2) // 2009-11-12
```

### How to check a date in a range?
The operators `<`, `>` are not defined to compare date / time. Use `time.After()` and `time.Before()` instead. 
```
	s1 := "2020-07-19"
	s2 := "2020-09-01"
	t1, _ := time.Parse("2006-01-02", s1)
	t2, _ := time.Parse("2006-01-02", s2)

    // invalid operation: t1 < t2 (operator < not defined on struct)
	// fmt.Println(t1 < t2)

	fmt.Println(t1.After(t2), t1.Before(t2)) // false true
```

## Template
### How to use `if` and `eq` in template?
```
{{if eq .Lang "tw"}}
{{else if eq .Lang "cn"}}
{{else if eq .Lang "en"}}
{{end}}
```

### How to use `or` in template?
```
{{if or (eq .Lang "tw") (eq .Lang "cn")}}
```

## Module
### How to view available dependency upgrades
`go list -u -m all`

### How to update all the dependencies to the latest version?
`go get -u ./...`

## Troubleshooting
### could not launch process
```
C:\>dlv debug
could not launch process: decoding dwarf section info at offset 0x0: too short
could not remove C:\Users\Andrew\Documents\Andrew\debug: remove C:\Users\Andrew\Documents\Andrew\debug: The process cannot access the file because it is being used by another process.

C:\>dlv version
Delve Debugger
Version: 1.0.0
Build: $Id: c98a142125d0b17bb11ec0513bde346229b5f533 $

C:\>where dlv
C:\andrew\prj\go\bin\dlv.exe // old version 1.0 doesn't work with Go 1.4
C:\Users\Andrew\go\bin\dlv.exe // new version 1.5 doesn't run
```
Both Delve and Go versions must compatible for it to work.
Remove the old version and update both Delve and Go to their latest version will solve the problem. 

## Pitfalls
### How to convert number to string?
```
// ASCII code
s := string(65) // convert to "A"

// Right
n1 := int64(234)
s1 := strconv.FormatInt(n1, 10) // "234"

n2 := 4.56423
s2 := strconv.FormatFloat(n2, 'f', 2, 32) // "4.56"
```


