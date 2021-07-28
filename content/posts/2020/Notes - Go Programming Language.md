---
title: "Notes - Go Programming Language"
date: 2020-07-31T14:47:26-07:00
categories:
 - Tech
 - Programming
tags:
 - Go
draft: false
---

## JSON
### How to process NaN value?
When liquidation and total equals zero, the `item.LIQRate` will be `NAN`.
```
item.LIQRate = float64(item.Liquidation) / float64(item.Total)
```
`json.Marshal(item)` will throw this error:
```
json: unsupported value: NaN
```

### Should I parse JSON with structs or maps?
#### Struct
Pro: 
* Parsing JSON data to structs is safe and easy.

Con: 
* When using structs, we must define every element within the JSON into the struct. 
We have to know the field name and data type of each JSON element while writing our code.
```go
    / Example is our main data structure used for JSON parsing
    type Example struct {
        Name    string `json:"name"`
        Numbers []int  `json:"numbers"`
        Nested  Nested `json:"nested"`
    }
    
    // Nested is an embedded structure within Example
    type Nested struct {
        IsIt        bool   `json:"isit"`
        Description string `json:"description"`
    }
    
    func main() {
        // Define a JSON string
        j := `{"name":"example","numbers":[1,2,3,4],"nested":{"isit":true,"description":"a nested json"}}`
    
        // Parse JSON string into data object
        var data Example
        err := json.Unmarshal([]byte(j), &data)
        if err != nil {
            fmt.Printf("Error parsing JSON string - %s", err)
        }
    
        // Print the name
        fmt.Printf("Name is %s", data.Name)
    }
```  

#### Map
Pro: 
* Using map when we need to parse an unknown JSON.
* Easy for simply data struct: `data["name"].(string)`; but it may become a mess rapidly if you want to check if the map missing the key or if the type is wrong.

Con: 
* Using `map[string]interface{}` is generally unsafe and require extra work to use the data safely once parsed.

```go
    var data map[string]interface{}

	// Define a JSON string
	j := `{"name":"example","numbers":[1,2,3,4],"nested":{"isit":true,"description":"a nested json"}}`

	// Parse our JSON string
	err := json.Unmarshal([]byte(j), &data)
	if err != nil {
		fmt.Printf("Error parsing JSON string - %s", err)
	}
	
	// Print out one of our JSON values
	n, ok := data["name"]
	if !ok {
		// access it another way
		n = "defaultName"
	}
	v, ok := n.(string)
	if !ok {
		// figure out type another way
		v = "defaultValue"
	}
	fmt.Printf("Name is %s", v)
	
	// Print out the JSON Numbers
	var nums []int
	i, ok := data["numbers"].([]interface{})
	if ok {
		for _, v := range i {
			x, ok := v.(float64)
			if !ok {
				// set to default
				nums = []int{}
				break
			}
			nums = append(nums, int(x))

		}
	}
	fmt.Printf("Numbers are")
	for _, v := range nums {
		fmt.Printf(" %d", v)
	}
	fmt.Printf("\n")
```

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
## CSV
### How to remove BOM data at the beginning of the file
```go
	f, _ := os.Open(`RL Inventory All Fields_12012021_Vancouver, BC (RL).csv`)
	defer f.Close()

	r := csv.NewReader(f)
    row, _ := r.Read() // one record (a slice of fields)
    for i, col := range row {
        if i == 0 {
            // See https://github.com/golang/go/issues/33887
            bom:="\xEF\xBB\xBF"
            bom2:="\uFEFF" // same as "\xEF\xBB\xBF" above
            fmt.Println(strings.Contains(v,bom)) // true
            fmt.Println(strings.Contains(v,bom2)) // true
            fmt.Println(strings.TrimLeft(v,bom)) // Facility
            break
        }
    }
```

### How to set Comma separator?
```go
    r := csv.NewReader(strings.NewReader(my-csv-string))
	r.Comma = '#'
	r.Comment = '*'
```
Note: Comma and Comment must be a valid rune, not string. For example, if using `r.Comma = "#"` will cause this error:
```
cannot use "#" (type untyped string) as type rune in assignment
```

## Context

### WithCancel
```go
ctx := context.Background()
ctx, cancel := context.WithCancel(ctx)
// defer cancel()
// cancel after 2 seconds
time.AfterFunc(2*time.Second, cancel) // continue to the next line, doesn't wait
doWithCtx(ctx, 5*time.Second) // context canceled: abort after 2 seconds
// context canceled
```
```go
func doWithCtx(ctx context.Context, d time.Duration) {
	select {
	// cancelled
	case <-ctx.Done():
		log.Println(ctx.Err())
	case <-time.After(d):
		fmt.Println("Done")
	}
}
```

### WithTimeout
```go
ctx := context.Background()
ctx, cancel := context.WithTimeout(ctx, time.Second)
defer cancel()

doWithCtx(ctx, 5*time.Second) // context deadline exceeded
```

## Time

### How long the process last?
```go
start := time.Now()
doSth()
end := time.Now()
fmt.Printf("time last %v\n", end.Sub(start))
```

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

### Unix Time
```go
    ut := time.Now().Unix() 
    fmt.Println(ut) 
    // output: 1257894000
    
    fmt.Println(time.Unix(ut, 0)) 
    // output: 2009-11-10 23:00:00 +0000 UTC
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
### Go Install
Usually you need to use `go get` to download and install a package. 
Use `go install` when you want to install an executable program(cmd - main package).

The go install command builds and installs the packages named by the paths on the command line. 
Executables (main packages) are installed to the directory named by the `GOBIN` environment variable, 
which defaults to `$GOPATH/bin` or `$HOME/go/bin` if the GOPATH environment variable is not set. 
Executables in `$GOROOT` are installed in `$GOROOT/bin` or `$GOTOOLDIR` instead of `$GOBIN`.

Since Go 1.16, if the arguments have version suffixes (like @latest or @v1.0.0), go install builds packages in module-aware mode, 
ignoring the go.mod file in the current directory or any parent directory if there is one. 
This is useful for installing executables without affecting the dependencies of the main module.
```
# Install the latest version of a program,
# ignoring go.mod in the current directory (if any).
$ go install golang.org/x/tools/gopls@latest

# Install a specific version of a program.
$ go install golang.org/x/tools/gopls@v0.6.4

# Install a program at the version selected by the module in the current directory.
$ go install golang.org/x/tools/gopls

# Install all programs in a directory.
$ go install ./cmd/...
```

### Useful Commands:
```
go get github.com/gin-gonic/gin // install latest published module
go get -u github.com/gin-gonic/gin // update the module

// Version queries
go get github.com/gin-gonic/gin@master // get the latest module 

// update go version to version 1.16
go mod edit -go=1.16
```

### Module and GOPATH
If the environment variable is unset, GOPATH defaults to a subdirectory named "go" in the user's home directory 
($HOME/go on Unix, %USERPROFILE%\go on Windows), unless that directory holds a Go distribution. 
Run "go env GOPATH" to see the current GOPATH.

When using modules, GOPATH is no longer used for resolving imports. 
However, it is still used to store downloaded source code (in GOPATH/pkg/mod) and compiled commands (in GOPATH/bin).

### How to clean up the modules?
```
go mod tidy // add missing modules and remove unused modules
```

### How to upgrade to a specific commit?
```
C:\Users\angan\go\src\rl>go get -u github.com/mxschmitt/playwright-go@355fba9
go: downloading github.com/mxschmitt/playwright-go v0.171.2-0.20210220003257-355fba93c781
go: github.com/mxschmitt/playwright-go 355fba9 => v0.171.2-0.20210220003257-355fba93c781
```
### How to upgrade all dependencies at once?
```
go list -u -m all // View available dependency upgrades
go get -u ./... // update all the dependencies
```

### Should I commit `go.sum` file as well as my `go.mod` file to Github?
Yes. See [here](https://github.com/golang/go/wiki/Modules#should-i-commit-my-gosum-file-as-well-as-my-gomod-file)

## Troubleshooting
### Error: go get .: path is not a package in module rooted
```
C:\Users\angan\go\src\rl>go get -u
go get .: path C:\Users\angan\go\src\rl is not a package in module rooted at C:\Users\angan\go\src\rl
```
See How to upgrade all dependencies at once?

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

## Interface
### Interface Type assertion
#### How to convert interface to string?
String Example:
```go
// stats is interface
stats,_ := page.EvalOnSelector("//div[text()='Employee Name']/../../..",f)
// convert interface to string
r:=csv.NewReader(strings.NewReader(stats.(string)))
```

#### How to convert interface to float64?
```
var data interface{} // nil
data = float64(3.4)
f := data.(float64)
a, ok := data.(float64)
if !ok {
    // data is not float64
    ...
}    
```

### Type assertion or Parse float64?
```go
var i interface{}
i = "24.08"

// incorrect
// ok is always false; i is string type
f, ok := i.(float64) 

// correct
// first convert i to string, then use parse float function
threshold, err := strconv.ParseFloat(i.(string), 64)
```

### Type switch
example from [here](https://stackoverflow.com/questions/18041334/convert-interface-to-int)

```
func main() {
    var val interface{}
    val = 4

    var i int

    switch t := val.(type) {
    case int:
        fmt.Printf("%d == %T\n", t, t)
        i = t
    case int8:
        fmt.Printf("%d == %T\n", t, t)
        i = int(t) // standardizes across systems
    case int16:
        fmt.Printf("%d == %T\n", t, t)
        i = int(t) // standardizes across systems
    case int32:
        fmt.Printf("%d == %T\n", t, t)
        i = int(t) // standardizes across systems
    case int64:
        fmt.Printf("%d == %T\n", t, t)
        i = int(t) // standardizes across systems
    case bool:
        fmt.Printf("%t == %T\n", t, t)
        // // not covertible unless...
        // if t {
        //  i = 1
        // } else {
        //  i = 0
        // }
    case float32:
        fmt.Printf("%g == %T\n", t, t)
        i = int(t) // standardizes across systems
    case float64:
        fmt.Printf("%f == %T\n", t, t)
        i = int(t) // standardizes across systems
    case uint8:
        fmt.Printf("%d == %T\n", t, t)
        i = int(t) // standardizes across systems
    case uint16:
        fmt.Printf("%d == %T\n", t, t)
        i = int(t) // standardizes across systems
    case uint32:
        fmt.Printf("%d == %T\n", t, t)
        i = int(t) // standardizes across systems
    case uint64:
        fmt.Printf("%d == %T\n", t, t)
        i = int(t) // standardizes across systems
    case string:
        fmt.Printf("%s == %T\n", t, t)
        // gets a little messy...
        // use strconv.ParseInt()
    default:
        // what is it then?
        fmt.Printf("%v == %T\n", t, t)
    }

    fmt.Printf("i == %d\n", i)
}
```

## IO
### How to show message on screen and write into file at the same time?
```go
// You must open the file in read/write mode
f, err := os.OpenFile("app.log", os.O_RDWR|os.O_CREATE|os.O_APPEND, 0666)
if err != nil {
    log.Fatalf("error opening file: %v", err)
}
defer f.Close()

wrt := io.MultiWriter(os.Stdout, f)
log.SetOutput(wrt)

log.Println("Hello, World!")
```

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

### How to check if file exist?


### How to copy file?
```
// Method 1
src, _ := os.Open(srcFile)
defer src.Close()

dst, _ := os.Create(dstFile)
defer dst.Close()

n, err := io.Copy(dst, src)

// Method 2
src, _ := ioutil.ReadFile(srcFile)
_ = ioutil.WriteFile(dstFile, src, 0644)

// Method 3
buf := make([]byte, BUFFERSIZE)
for {
    n, err := src.Read(buf)
    if err != nil && err != io.EOF {
        return err
    }
    if n == 0 {
        break
    }
    if _, err := dst.Write(buf[:n]); err != nil {
        return err
    }
}
```

## Network
### Simple Go client
Get Content
```go
    response, _ := http.Get(url)
	defer response.Body.Close()

	if response.StatusCode != 200 {
		...
	}

	b, _ := ioutil.ReadAll(response.Body)
	fmt.Println(string(b))
```

Download file:
```go	
	file, _ := os.Create(fileName)
	defer file.Close()

	//Write the bytes to the fiel
	_ = io.Copy(file, response.Body)
```

### Customized Go Client
```go
type egnyteFile struct {
	Path string
	Name string
}
type egnyteResponse struct {
	Name  string
	Path  string
	Files []egnyteFile
}

func main() {
	url := "https://mydomain.egnyte.com/pubapi/v1/fs/xxx"
	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("Authorization", "Bearer "+"xxx")
	client := &http.Client{}
	response, _ := client.Do(req)
	defer response.Body.Close()

	if response.StatusCode != 200 {
		log.Fatal(response.StatusCode)
	}

	var e egnyteResponse
	json.NewDecoder(response.Body).Decode(&e)

	for _, v := range e.Files {
		if strings.Contains(v.Name, "Vancouver, BC") {
			fmt.Println(v.Path)
			break
		}
	}
}
```

## Other
### How to show your library in GODOC?

* By using GODOC command line

See [here](https://pkg.go.dev/golang.org/x/tools/cmd/godoc)
```
// Note: install the godoc cmd
go get -v  golang.org/x/tools/cmd/godoc

// go to the root of your library
godoc -http=:6060
```

* By using https://pkg.go.dev/

## Useful links
* [Cobra](https://github.com/spf13/cobra) is a library providing a simple interface to create powerful modern CLI interfaces
* [Echo](https://echo.labstack.com) is a high performance, extensible, minimalist Go web framework
* [Package validator](https://github.com/go-playground/validator) implements value validations for structs and individual fields based on tags.