---
title: "Notes - Protocol Buffers"
date: 2022-05-30T11:04:30-07:00
categories:
  - Tech
  - Programming
tags:
  - Go
  - Protocol Buffers
  - JSON
draft: false
---

Protocol buffers provide a language-neutral mechanism for serializing structured data. 
It’s like JSON, except it's smaller and faster.

## Step by step
```
mkdir pb
cd pb
go mod init pb
```

### Create the `people.proto` file
```
syntax="proto3";
package tutorial;

option go_package = "./tutorial";

message Person{
  string name=1;
  int32 id=2;
}
```
### Download compiler
* Download the `protoc-21.1-win64.zip` compiler from [https://github.com/protocolbuffers/protobuf](https://github.com/protocolbuffers/protobuf/releases)
* Unzip the files into a folder and copy `protoc.exe` into $GOBIN

### Install `protoc-gen-go`
To generating Go package, you need to install Go protocol buffers plugin:
```
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
```

### Generating code
```
protoc --go_out=. *.proto
```

### Use the generated package
```go
package main

import (
	"fmt"
	"google.golang.org/protobuf/proto"
	"log"
	"pb/tutorial"
)

func main() {
	p := &tutorial.Person{
		Name: "Andrew",
		Id:   0,
	}

	data, err := proto.Marshal(p)
	if err != nil {
		log.Fatalln(err)
	}
	fmt.Println(data)

	np := &tutorial.Person{}
	err = proto.Unmarshal(data, np)
	if err != nil {
		log.Fatalln(err)
	}
	fmt.Println(np)

}
```

### How to use `timestamp.proto`?
* Import it into your `.proto` file
```go
syntax = "proto3";
package tutorial;

import "google/protobuf/timestamp.proto";
...
```

* Copy `include` folder(in `protoc-21.1-win64.zip`) into your project folder(or anywhere)
* Specify the folder when compile by using `--proto_path=./include`
```
protoc --go_out=.  --proto_path=./include --proto_path=. *.proto  
```

## Troubleshooting
### Go package can't be a plain text
```
// in people.proto
option go_package = "tutorial" // failed to compile

// when compile
pb> protoc --go_out=. *.proto
protoc-gen-go: invalid Go import path "tutorial" for "people.proto"

The import path must contain at least one period ('.') or forward slash ('/') character.
```

Solution: change the package name from "tutorial" to "./tutorial"

## Useful Links
* [Protocol Buffer Language Guide - proto3](https://developers.google.com/protocol-buffers/docs/proto3)
* [Protocol Buffer Style Guide](https://developers.google.com/protocol-buffers/docs/style)
* [Go API Reference](https://pkg.go.dev/google.golang.org/protobuf)
* [Go support for Google's protocol buffers](https://pkg.go.dev/google.golang.org/protobuf)