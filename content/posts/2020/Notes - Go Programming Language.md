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

## Standard Library

### How to re-use parameters in `fmt.Printf()`?
See [fmt](https://pkg.go.dev/fmt@go1.18.2#hdr-Explicit_argument_indexes) 

In Printf, Sprintf, and Fprintf, the default behavior is for each formatting verb to format successive arguments passed in the call. 
However, the notation [n] immediately before the verb indicates that the nth one-indexed argument is to be formatted instead. 
The same notation before a '*' for a width or precision selects the argument index holding the value. 
After processing a bracketed expression [n], subsequent verbs will use arguments n+1, n+2, etc. unless otherwise directed.
```
fmt.Printf("%s, %[1]s","echo") // echo, echo
fmt.Printf("%v-%v-%v-%[2]v-%v", 100, 200, 300) // 100-200-300-200-300
```

## JSON
### JSON Name Convention
The JSON syntax does not impose any restrictions on the strings used as names.
Most of them either use camelCase or snake_case.

For Go we usually use camelCase like this example code(`myName`) in json package:
```
// Field appears in JSON as key "myName".
Field int `json:"myName"`
```

### What's the JSON backslash escapes?
You need to use backslash(`\`) to escape the following characters:
* `\\`: backslash
* `\"`: quotation mark
* `\b`: backspace
* `\f`: formfeed
* `\n`: linefeed
* `\r`: carriage return
* `\t`: horizontal tab
* `\uxxxx`: 
* `\/`: slash (optional: The JSON spec says you CAN escape forward slash, but you don't have to. This helps when embedding JSON in a `<script>` tag, which doesn't allow `</` inside strings)

### How to get JSON info from request?
```go
info := Info{}
defer r.Body.Close()

// 1. steam
err := json.NewDecoder(r.Body).Decode(&info)

// 2.
data, err := ioutil.ReadAll(r.Body)
err = json.Unmarshal(data, &info)
```

### Error when processing NaN value
When liquidation and total equals zero, the `item.LIQRate` will be `NAN`.
```
item.LIQRate = float64(item.Liquidation) / float64(item.Total)
```
`json.Marshal(item)` will throw this error:
```
json: unsupported value: NaN
```
There's a [ticket](https://github.com/golang/go/issues/3480) for it.

In this case, it's better to preprocess the NAN to 0 before marshal it or you can use 

### How to customize JSON?
You can use UnmarshalJSON

Here is an example to convert JSON array `["1", "Andrew"]` into Go type Associate {"ID": 1, "Name": "Andrew"}
```
type Associate struct {
	ID   int
	Name string
}

func (a *Associate) UnmarshalJSON(data []byte) error {
	var rowValue []string
	if err := json.Unmarshal(data, &rowValue); err != nil {
		return err
	}
	id, err := strconv.Atoi(rowValue[0])
	if err != nil {
		return err
	}
	a.ID = id
	a.Name = rowValue[1]
	// fmt.Println(rowValue)
	return nil
}

func main() {
	data := []byte(`[
	["1", "Andrew"],
	["2", "John"],
	["3", "Vivian"]
]`)
	var associates []Associate
	err := json.Unmarshal(data, &associates)
	if err != nil {
		log.Fatalln(err)
	}

	fmt.Println(associates)
}
// output
[{1 Andrew} {2 John} {3 Vivian}]
```

### How to reuse original type?
See [here](http://choly.ca/post/go-json-marshalling/)

Example on how to change `LastSeen` field as a unix timestamp by using alias to embed the original type `User`
```
type User struct {
	ID       int64     `json:"id"`
	Name     string    `json:"name"`
	LastSeen time.Time `json:"lastSeen"`
}

func (u *User) UnmarshalJSON(data []byte) error {
	type Alias User
	aux := &struct {
		LastSeen int64 `json:"lastSeen"`
		*Alias
	}{
		Alias: (*Alias)(u),
	}
	if err := json.Unmarshal(data, &aux); err != nil {
		return err
	}
	u.LastSeen = time.Unix(aux.LastSeen, 0)
	return nil
}
```
### Should I parse JSON with structs or maps?
See [here](https://bencane.com/2020/12/08/maps-vs-structs-for-json/)

#### Using Struct
Pro: 
* Parsing JSON data to structs is safe and easy.

Con: 
* When using structs, we must define every element within the JSON into the struct. 
We have to know the field name and data type of each JSON element while writing our code.
```go
    // Example is our main data structure used for JSON parsing
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

#### Using Map
I use it when reading the JSON config file in a program. 

Pro: 
* Using map when we need to parse an unknown JSON.
* Easy for simply data struct: `data["name"].(string)`; 
  but it may become a mess rapidly if you want to check if the map missing the key or if the type is wrong.

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

## Flag
### How to specify flag at command line?
The following forms are permitted:
```
-flag
-flag=x
-flag x  // non-boolean flags only
```
One or two minus signs may be used; they are equivalent.

For example:
```go
  // foo.exe
  var message string
  flag.StringVar(&message, "message", "default", "The of the user metrics. Default date is today. Date format: YYYY-MM-DD(2022-01-28)")
  flag.Parse()
  fmt.Println(message)
  
  // foo.exe => default
  // foo.exe -message 
```


## CSV
### How to check BOM data at the beginning of the file
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

### How to include special characters in CSV?
* If there is a comma in the text, you can quote the field like `"Guo, Angang"`
* If there is a quote in the text, quote it like `"""Q"""` for `"Q"`

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

### Using embedded template files
See "Embed Static Files" section for details

## Embed Static Files
### Three methods to embed a file
1. As string
```
import _ "embed"

//go:embed metrics.html
var tmplMetricFile string

var tmpls = template.Must(template.New("metrics.html").Parse(tmplMetricFile))
```

2. As []byte
```
import _ "embed"

//go:embed home.html
var tmplHomeFile []byte

var tmplHome = template.Must(template.New("home.html").Parse(tmplHomeFile))
```

3. As FS
```
import "embed"

// single file
//go:embed index.html
var tmplFS embed.FS
template.Must(template.ParseFS(tmplFS, "index.html"))

// or embed multiple directories and files
//go:embed template static config.json
var content embel.FS
template.Must(template.ParseFS(content, "template/*"))
```

See [embed package](https://pkg.go.dev/embed)

### Base template, and Template
* Base Template

Prefer to define the **whole content** in the base file as a template. 
For example, base.gohtml define the whole file as **"tmBase"**
```
{{define "tmBase"}}
<!doctype html>
<html lang="en">
<head>
    <title>
      {{block "tmTitle" .}}No Title{{end}}
    </title>
</head>
<body>
    {{block "tmContent" .}}No content{{end}}
    {{block "tmScript" .}}{{end}}
</body>
</html>
{{end}}
```

#### Method# 1
Other files will use the base template and define other blocks in the file

```
// index.gohtml
{{template "tmBase" .}}
{{define "tmTitle"}}Index Title{{end}}
{{define "tmContent"}} <h2>Index Content</h2> {{end}}

// Note: base.gohtml must be list as the first file in ParseFiles, then other files
tmpl, _ := template.ParseFiles("template/base.gohtml", "template/index.gohtml")
tmpl.ExecuteTemplate(os.Stdout, "index.gohtml", "data")
```

#### Method# 2 
1. Don't use `{{template "tmBase" .}}` in content template `index.gohtml`
2. Use "tmBase" template name instead of file name "index.gohtml" in `ExecuteTemplate`

```
// index.gohtml
// 1. remove this line: {{template "tmBase" .}}
{{define "tmTitle"}}Index Title{{end}}
{{define "tmContent"}} <h2>Index Content</h2> {{end}}

tmpl, _ := template.ParseFiles("template/base.gohtml", "template/index.gohtml")
// 2. use "tmBase" template instead of "index.gohtml"
tmpl.ExecuteTemplate(os.Stdout, "tmBase", "data")
```

* Pitfall: you can only parse base and one of the content template. 
If you parse more than one content template files, the last one will override the previous content.

```
// base.gohtml
{{define "tmBase"}}
base beginning
{{block "tmTitle" .}}Default base Title{{end}}
base ending
{{end}}

// first.gohtml
{{template "tmBase" .}}
{{define "tmTitle"}}First: {{.}} {{end}}

// last.gohtml
{{template "tmBase" .}}
{{define "tmTitle"}}Last: {{.}}{{end}}

tmpl, _ := template.ParseFiles("base.gohtml", "first.gohtml", "last.gohtml")
tmpl.ExecuteTemplate(os.Stdout, "index.gohtml", "data")

// Output: 
// Incorrect!!!
base beginning
Last: data // last override previous first data
base ending
```

### Template Name for ParseFiles
* Default template name is the first file name

```
// Prefer to use default file name and ExecuteTemplate to specify the template name as shown in this example
tmpl, _ := template.ParseFiles("template/base.gohtml", "template/index.gohtml")
fmt.Println(tmpl.Name()) // base.gohtml
tmpl.ExecuteTemplate(os.Stdout, "index.gohtml", data)
```

* If you want to specify a name for the template, you must use one of the file name

From [ParseFiles doc](https://pkg.go.dev/text/template@go1.18.2#Template.ParseFiles):

Since the templates created by `func (t *Template) ParseFiles(filenames ...string) (*Template, error)` are named 
by the base names of the argument files, it should usually have the name of one of the (base) names of the files.

```
// the name must be either "index.gohtml" or "base.gohtml", 
// Not recommanded
temp, err := template.New("index.gohtml").ParseFiles("template/base.gohtml", "template/index.gohtml")
```

* It will be an error if you use a name other than the listed file names

```
indexTmpl, _ := template.New("aa").ParseFiles("template/base.gohtml", "template/index.gohtml")
indexTmpl.Execute(os.Stdout, nil) // or indexTmpl.ExecuteTemplate(os.Stdout, "aa", nil)
if err != nil {
    log.Fatal(err)
    // 2022/05/17 21:28:04 error: html/template: "aa" is an incomplete template
}
```

## Module
See [Developing and publishing modules](https://golang.org/doc/modules/developing)

### How to replace a module?
The Go Excel library has changed the location and path from `github.com/360EntSecGroup-Skylar/excelize` to `github.com/xuri/excelize`.
Our program all using the import path as `"github.com/360EntSecGroup-Skylar/excelize/v2"`

When updating the modules, the following error occurs:
```
PS C:\Andrew\prj\lib> go get -u ./...
go get: github.com/360EntSecGroup-Skylar/excelize/v2@v2.4.0 updating to
        github.com/360EntSecGroup-Skylar/excelize/v2@v2.4.1: parsing go.mod:
        module declares its path as: github.com/xuri/excelize/v2
                but was required as: github.com/360EntSecGroup-Skylar/excelize/v2
```

Use `replace` directive will fix it:
```go
replace github.com/360EntSecGroup-Skylar/excelize/v2 => github.com/xuri/excelize/v2 v2.4.1

require (
	github.com/360EntSecGroup-Skylar/excelize/v2 v2.4.1
)
```

### How to use unpublished module in your local directory?
Use `replace` directive as the following:
```go
module example.com/mymodule

go 1.16

require example.com/theirmodule v0.0.0-unpublished
replace example.com/theirmodule v0.0.0-unpublished => ../to-local-folder

// $ go mod edit -replace=example.com/theirmodule@v0.0.0-unpublished=../theirmodule
// $ go get -d example.com/theirmodule@v0.0.0-unpublished
```

### Useful Commands:
```
go get github.com/gin-gonic/gin // install latest published module
go get -u github.com/gin-gonic/gin // update the module

// Version queries
go get github.com/gin-gonic/gin@master // get the latest module 

go list -u -m all // View available dependency upgrades
go get -u ./... // update all the dependencies

// upgrade to a specific commit
go get -u github.com/mxschmitt/playwright-go@355fba9
go: downloading github.com/mxschmitt/playwright-go v0.171.2-0.20210220003257-355fba93c781
go: github.com/mxschmitt/playwright-go 355fba9 => v0.171.2-0.20210220003257-355fba93c781

// update go version to version 1.16
go mod edit -go=1.16

// who's using this module
go mod why -m gopkg.in/yaml.v2
Output:
# gopkg.in/yaml.v2
github.com/AngangGuo/rl/archived/stats
github.com/gin-gonic/gin
github.com/gin-gonic/gin/binding
gopkg.in/yaml.v2

// clean up the modules; add missing modules and remove unused modules
go mod tidy

```

### Module and GOPATH
If the environment variable is unset, GOPATH defaults to a subdirectory named "go" in the user's home directory 
($HOME/go on Unix, %USERPROFILE%\go on Windows), unless that directory holds a Go distribution. 
Run "go env GOPATH" to see the current GOPATH.

When using modules, GOPATH is no longer used for resolving imports. 
However, it is still used to store downloaded source code (in GOPATH/pkg/mod) and compiled commands (in GOPATH/bin).

### Should I commit `go.sum` file as well as my `go.mod` file to Github?
Yes. See [here](https://github.com/golang/go/wiki/Modules#should-i-commit-my-gosum-file-as-well-as-my-gomod-file)

## Tooling
### Build Constraints
See [doc](https://pkg.go.dev/cmd/go#hdr-Build_constraints)

Build constraint(or build tag) is a line comment that begins(starting go1.17)
```
//go:build
```
It appears near the top of the file, preceded only by blank lines and other line comments.
A build constraint is evaluated as an expression containing options combined by ||, &&, and ! operators and parentheses.
Here're some examples:
* To keep a file from being considered for the build(first line is used after Go1.17, second line is used before Go1.17):
```
//go:build ignore
// +build ignore
```

* To build a file only when using cgo, and only on Linux and OS X:
```
//go:build cgo && (linux || darwin)
```

Note: Go versions 1.16 and earlier used a different syntax for build constraints, with a `// +build` prefix.

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

## Troubleshooting
### Error: go get .: path is not a package in module rooted
```
C:\Users\angan\go\src\rl>go get -u
go get .: path C:\Users\angan\go\src\rl is not a package in module rooted at C:\Users\angan\go\src\rl
```
See How to upgrade all dependencies at once?

### `dlv`: could not launch process
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
### How to read string from command line?
```go
scanner := bufio.NewScanner(os.Stdin)
log.Println("Enter user name: ")
scanner.Scan()
username := scanner.Text()
fmt.Printf("Your name is %s", username)
```

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

### How to check if file exist?
```go
if _, err := os.Stat("/path/to/whatever"); err == nil {
  // exists
} else if os.IsNotExist(err) { // or errors.Is(err, fs.ErrNotExist)
  // does not exist
} else {
  // file may or may not exist. See err for details.
  // permission denied, 
}
```

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

### How to properly close the request body?
```go
defer func() {
    err := r.Body.Close()

    if err != nil {
        http.Error(w, err.Error(), 500)
        return
    }
}()
```

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

### How to response with message continually?
```
func HandlePost(w http.ResponseWriter, r *http.Request) {
    // #1 add flusher
    flusher, ok := w.(http.Flusher)
    if !ok {
        panic("expected http.ResponseWriter to be an http.Flusher")
    }
    w.Header().Set("Connection", "Keep-Alive")
    
    // make sure this header is set
    w.Header().Set("X-Content-Type-Options", "nosniff")

    ticker := time.NewTicker(time.Millisecond * 500)
    go func() {
        for t := range ticker.C {
            // #2 add '\n'
            io.WriteString(w, "Chunk\n")
            flusher.Flush()
        }
    }()
    time.Sleep(time.Second * 5)
    ticker.Stop()
}
```

See [here](https://stackoverflow.com/questions/26769626/send-a-chunked-http-response-from-a-go-server)

## Document
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

### How can I add examples into my library document?
See [blog](https://blog.golang.org/examples)

## Slice
### Copy function usage
The built-in copy function copies elements into a destination slice dst from a source slice src.
```
func copy(dst, src []Type) int
```

As a special case, it’s legal to copy bytes from a string to a slice of bytes.
```
// copy(dst []byte, src string) int
a := [8]byte{}
b := "12345678"
copy(a[:], b)
fmt.Println(a) // [49 50 51 52 53 54 55 56]
```

## Struct
### How to compare with an empty struct?
```
type Coordinate struct {
	Row    int
	Column int
}

type Config struct {
	FirstDateCoordinates Coordinate `json:"first-date-coordinates"`
}

var c = Config{}
// 
fmt.Println(c.FirstDateCoordinates == (Coordinate{}))
```
Note:
Because of a parsing ambiguity, parentheses are required around the composite literal `Coordinate{}`.

The use of `==` above applies to structs where all fields are comparable. 
If the struct contains a non-comparable field (slice, map or function), 
then the fields must be compared one by one to their zero values.

## Map
### `nil` Map
A gotcha with maps is that they can be a nil value. A nil map behaves like an empty map when reading, 
but attempts to write to a nil map will cause a runtime panic. You can read more about maps here.

Therefore, you should never initialize an empty map variable:
```go
// avoid this 
var m map[string]string

// use this instead
var dictionary = map[string]string{}
// or this
var dictionary = make(map[string]string)
```

### Pass By Value
when you pass a map to a function/method, you are indeed copying it, 
but just the pointer part(it's a pointer to the underlying runtime.hmap structure), 
not the data structure that contains the data. So you can modify the map data without passing the map address.
```go
func main() {
  myMap := map[string]bool{}
  fmt.Println("before", myMap)
  
  change(myMap)
  fmt.Println("after", myMap)
}

func change(m map[string]bool) {
  m["modified"] = true
}

// output:
before map[]
after map[modified:true]
```

## Concurrency
### Once
`func (o *Once) Do(f func())`

Tip: if once.Do(f) is called multiple times, only the first call will invoke f, 
even if f has a different value in each invocation.

```go
var count int
increment := func() { count++ }
decrement := func() { count-- }
var once sync.Once
once.Do(increment)
once.Do(decrement)
fmt.Printf("Count: %d\n", count)

// output:
// Count: 1
```

### How to check if channel is closed?
You can always read values from a closed channel. 
It will return the default value of the channel type.

To check if a channel is closed:
```go
v, ok <- myChan
if !ok {
  fmt.Println("chan is closed")
}  
```
## Pitfalls

### Break!!
From [Spec](https://go.dev/ref/spec#Break_statements): 
A "break" statement terminates execution of the **innermost** "for", "switch", or "select" statement within the same function.

```
interval := time.NewTicker(5 * time.Minute)
var nextUpdateTime time.Time = time.Now()
for {
    select {
    case <-interval.C:
        if time.Now().After(nextUpdateTime) {
            doJob()
            nextUpdateTime = getSchedule()
        }
    case <-done:
        break // Wrong!!
    }
}
log.Println("job finished")		
```
A bare `break` can only get out of the inside `select` statement, not the outside `for` statement. 
You need to use `break label` to terminate the `for` loop. See the example below.

### Why loop three times?
```go
func main() {
	count:=0
	c := make(chan int)
	go func() {
		time.Sleep(5 * time.Second)
		close(c)
	}()

loop:
	for {
		select {
		case <-c:
			break loop
		case <-time.After(time.Second):
			break loop
		default:
			count++
			time.Sleep(2*time.Second)
		}
	}

	fmt.Println(count) // 3
}
```

Because the `time.After(time.Second)` is evaluated every time, the `default` section is executed immediately

### Missing value
Because calling a function makes a copy of each argument value, if a function needs to update
a variable, or if an argument is so large that we wish to avoid copy ing it, we must pass the
address of the variable using a pointer. The same goes for methods that need to update the
receiver variable.

```go
type Student struct {
	Name string
}

func NewStudent() Student {
	return Student{}
}
	
// Problem
func (s Student) SetName(name string) {
	s.Name = name
}

func main() {
	a := NewStudent()
	a.SetName("Andrew")
	
	fmt.Println(a) 
	// Where's Andrew??? 
	// Output: {}
}
```

To fix it, use the Pointer Receiver `func (s *Student) SetName(name string)` instead.

### How to convert number to string?
```
// ASCII code
s := string(65) // convert to "A"

// Convert number to string
n1 := int64(234)
s1 := strconv.FormatInt(n1, 10) // "234"

n2 := 4.56423
s2 := strconv.FormatFloat(n2, 'f', 2, 32) // "4.56"

// Using fmt.Sprintf()
n3 := 123.456
s3 := fmt.Sprintf("%v", n3) // "123.456"
```

### Build problem
`go build` may fail if you name your files with the name pattern like the following situation.

If a file's name, after stripping the extension and a possible _test suffix, matches any of the following patterns:
```
*_GOOS
*_GOARCH
*_GOOS_GOARCH
```
(example: source_windows_amd64.go) where GOOS and GOARCH represent any known operating system and architecture values respectively, 
then the file is considered to have an implicit build constraint requiring those terms 
(in addition to any explicit constraints in the file).

### Wrong version when missing patch number?
See [Module version numbering](https://golang.org/doc/modules/version-numbers) on how to version your module.

Module was tagged as v0.1100.0, later on after some commits tagged as v.01400 without patch number. 
When using `go get -u xxx` to update the module, it shows the module as `v0.1100.1` instead of `v0.1400`. 
See [issue here](https://github.com/mxschmitt/playwright-go/issues/190)

## Testing
### TDD With Go
See [Learn Go With Test](https://github.com/quii/learn-go-with-tests)

### Dependency Injection
See [Google Wire](https://go.dev/blog/wire)

Dependency injection (DI) is a style of writing code such that the dependencies of a particular object 
(or, in go, a struct) are provided at the time the object is initialized.

There are basically three types of dependency injection:
* Constructor injection: the dependencies are provided through a class constructor.
* Setter injection: the client exposes a setter method that the injector uses to inject the dependency.
* Interface injection: the dependency provides an injector method that will inject the dependency into any client passed to it. Clients must implement an interface that exposes a setter method that accepts the dependency.

### Mocking
* Mocking: You use mocking to replace real things you inject with a pretend version that you can control and inspect in your tests.

### Name Convention
* To write a new test suite, create a file whose name ends `_test.go`
* The function name serves to identify the test routine: `func TestXxx(*testing.T){...}`

### Package Name Strategy
You can only have one package in a folder(`go build` restriction, but doesn't mention in Go Spec), or use `[packageName]_test` for testing file which is an exception to this rule.
* If you use the same package name as your code, it's called white box testing, you can test private functions as well as public functions
* If you use `[packageName]_test` as package name, you need to import your code and can test public functions only. It's called black box testing.
* For testing package name strategy see [here](https://stackoverflow.com/questions/19998250/proper-package-naming-for-testing-with-the-go-language)

### Defining subtests with `Run`
Giving the individual test a name like `A=1` in this code:
```go
func TestFoo(t *testing.T) {
  // <setup code>
  t.Run("A=1", func(t *testing.T) { ... })
  t.Run("A=2", func(t *testing.T) { ... })
  t.Run("B=1", func(t *testing.T) { ... })
  // <tear-down code>
}
```

### Embedding `Run` and `Helper` example
Note that `t.Helper()` is needed in `assert` function to tell the test suite that this function is a helper. 
By doing this when it fails the line number reported will be in our function call rather than inside our test helper.

```go
func TestHello(t *testing.T) {
	assert:= func(t *testing.T, got, want string) {
		t.Helper()
		if got!=want{
		    // by using t.Helper(), error will be in line 26(caller position) - hello_test.go:26: got "Hello, world" want "Hello, world1"
		    // if you comment out the t.Helper(), error will be in line 9 - hello_test.go:9: got "Hello, world" want "Hello, world1"
			t.Errorf("got %q want %q", got, want)  // line 9
		}
	}

    // t.Run can be embeded
	t.Run("race", func(t *testing.T) {
		t.Run("chris", func(t *testing.T) {
			t.Parallel()
			got := Hello("Chris")
			want := "Hello, Chris"
			assert(t,got,want)
		})

		t.Run("empty", func(t *testing.T) {
			t.Parallel()
			got := Hello("")
			want := "Hello, world1"
			assert(t,got,want) // line 26 testing failed on this line
		})
	})
}

PS C:\Andrew\prj\try\tdd> go test
--- FAIL: TestHello (0.00s)
    --- FAIL: TestHello/race (0.00s)
        --- FAIL: TestHello/race/empty (0.00s)
            hello_test.go:25: got "Hello, world" want "Hello, world1"
FAIL
exit status 1
FAIL    github.com/AngangGuo/try/tdd 1.184s

```

### TestMain
```go
func TestMain(m *testing.M) {
	v := m.Run()
	if v == 0 && goroutineLeaked() {
		os.Exit(1)
	}
	os.Exit(v)
}

```

### Benchmarks
* To run the benchmarks, use `go test -bench .`
* Function of the form: `func BenchmarkXxx(*testing.B)`. For example
```go
func BenchmarkRandInt(b *testing.B) {
    // optional: reset timer after setup 
    // setup code
    // b.ResetTimer()
    for i := 0; i < b.N; i++ {
        rand.Int()
    }
}
```

### Coverage
You can use `go test -cover` to analyze the testing coverage.

### Examples
* For file name you can use `example_test.go`
* The naming convention to declare examples for the package, a function F, a type T and method M on type T are:
```
func Example() { ... }
func ExampleF() { ... }
func ExampleT() { ... }
func ExampleT_M() { ... }

// errors package example from go/src/errors/example_test.go
func Example() {
	if err := oops(); err != nil {
		fmt.Println(err)
	}
	// Output: 1989-03-15 22:30:00 +0000 UTC: the file system has gone away
}

// function strings.Join() example from go/src/strings/example_test.go
func ExampleJoin() {
	s := []string{"foo", "bar", "baz"}
	fmt.Println(strings.Join(s, ", "))
	// Output: foo, bar, baz
}

// type bytes.Buffer example from src/bytes/example_test.go
func ExampleBuffer() {
	var b bytes.Buffer // A Buffer needs no initialization.
	b.Write([]byte("Hello "))
	fmt.Fprintf(&b, "world!")
	b.WriteTo(os.Stdout)
	// Output: Hello world!
}

// type Buffer.Len method example from src/bytes/example_test.go
func ExampleBuffer_Len() {
	var b bytes.Buffer
	b.Grow(64)
	b.Write([]byte("abcde"))
	fmt.Printf("%d", b.Len())
	// Output: 5
}
```

Note:
If you use anything other than `Output:` or `Unordered output:`, the example will not compare with the standard output. 
No error message, it will just be treated as normal comment line.

* Multiple example functions for a package/type/function/method may be provided by appending a distinct suffix 
to the name. The suffix must start with a lower-case letter.
```go
// multiple bytes.Compare() function examples from src/bytes/example_test.go
func ExampleCompare() {...}
func ExampleCompare_search() {...)
```

## Other
### How to use text.tabwriter?
```go
wr := new(tabwriter.Writer)
// func (b *Writer) Init(output io.Writer, minwidth, tabwidth, padding int, padchar byte, flags uint) *Writer
wr.Init(os.Stdout, 1, 8, 1, '\t', 0)

fmt.Fprintln(wr,"Col-0\tCol-1\tCol-2\tCol-3\tCol-4")
fmt.Fprintln(wr,"1234\t12345678\t1234\t3\t4")

wr.Flush()
```
Output
```
Col-0   Col-1           Col-2   Col-3   Col-4
1234    12345678        1234    3       4
```

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


## Useful Library Links
* [Cobra](https://github.com/spf13/cobra) is a library providing a simple interface to create powerful modern CLI interfaces
* [Echo](https://echo.labstack.com) is a high performance, extensible, minimalist Go web framework
* [Validator](https://github.com/go-playground/validator) implements value validations for structs and individual fields based on tags.