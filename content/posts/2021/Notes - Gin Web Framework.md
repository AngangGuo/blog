---
title: "Notes - Gin Web Framework"
date: 2021-01-30T09:21:45-08:00
draft: true
---

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