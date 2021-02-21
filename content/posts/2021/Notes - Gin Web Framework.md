---
title: "Notes - Gin Web Framework"
date: 2021-01-30T09:21:45-08:00
draft: true
---

## How to send message to client?

### Render HTML Template
```go
router := gin.Default()

// loading all the templates
router.LoadHTMLGlob("templates/*")

// rendering template
router.GET("/", func(ctx *gin.Context) {
        // Call the HTML method of the Context to render a template
        ctx.HTML(
        // Set the HTTP status to 200 (OK)
        http.StatusOK,
        // Use the index.html template
        "index.html",
        // Pass the data that the page uses (in this case, 'title')
        gin.H{
        "title": "Home Page",
        },
    )
})
```
### Sending JSON
```go
ctx.JSON(http.StatusOK, gin.H{"message": "pong"})
```

### Sending String
```go
ctx.String(http.StatusOK, "Hello %s", name)
```

## How to get parameter/form values?
### URL Parameters
Gin uses [httprouter](https://github.com/julienschmidt/httprouter).

#### Named parameters
Named parameters(`:name`) only match a single path segment.
Since this router has only explicit matches, you can not register static routes and parameters for the same path segment.
For example if you registered `/user/:user` you can't register `/user/new` for the same request method at the same time.
```go
name := c.Param("name")
```
#### Catch-All parameters
Catch-All parameters(`*action`) match everything. Therefore they must always be at the end of the pattern
```go
action := c.Param("action")
```

### Querystring parameters
// single value: `http://localhost:8080/welcome?firstname=Jane&lastname=Doe`
```go
// DefaultQuery returns the query value if it exists, otherwise it returns the defaultValue.
firstname := c.DefaultQuery("firstname")

// shortcut for c.Request.URL.Query().Get("lastname")
lastname := c.Query("lastname")
```

// map: `http://localhost:8080/welcome?names[first]=Kevin&names[second]=Andrew`
```go
names := c.QueryMap("names")
```

### URLencoded/Multipart Form values
// Single value: `name=manu&notes=this_is_great`
```go
notes := c.PostForm("notes")
name := c.DefaultPostForm("name", "anonymous") 
```

// map: `names[first]=kevin&names[second]=andrew`
```go
names := c.PostFormMap("names")
```

## SSE(Server Sent Event) Example
Here is the simplest SSE server example.
```go
package main

import (
	"github.com/gin-gonic/gin"
	"io"
	"time"
)
func main() {
	r := gin.Default()

	// send SSE message to client infinitely
	r.GET("/stream", func(c *gin.Context) {
		c.Stream(func(w io.Writer) bool {
			time.Sleep(time.Second)
			c.SSEvent("message", time.Now().Format("2006-01-02 15:04:05"))
			return true
		})
	})

	// serve client page
	r.StaticFile("/", "./index.html")

	r.Run()
}
```

Here is the client file example:
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>SSE Example</title>
</head>
<body>

<script>
    const stream = new EventSource("/stream");
    stream.addEventListener("message", function (e) {
        document.body.innerHTML += e.data + "<br>";
    });
</script>
</body>
</html>
```
See [this reddit example](https://stackoverflow.com/questions/63078034/server-sent-event-unexpected-behavior)
or [this complex example](https://github.com/gin-gonic/examples/tree/master/server-sent-event) from Gin