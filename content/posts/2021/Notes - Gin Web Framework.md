---
title: "Notes - Gin Web Framework"
date: 2021-01-30T09:21:45-08:00
categories:
- Tech
- Web 
tags:
- Go
- Gin  
draft: false
---

## Router

### Full Router Examples
```go
func prepareRoutes(router *gin.Engine) {
	// Web Resources
	router.Static("/static", "web/dist")

	// API Routes
	api := router.Group("/api/v1")
	admin := api.Group("/admin")
	public := api.Group("/public")
	registered := api.Group("/")
	sameRegisteredUser := api.Group("/user")

	prepareMiddleware(admin, public, registered, sameRegisteredUser)

	admin.GET("/users", listUsers)
	admin.GET("/places", listPlaces)
	admin.POST("/places", createPlace)
	admin.POST("/events", createEvent)
	admin.PUT("/place/:placeId", updatePlace)
	admin.PUT("/event/:eventId", updateEvent)
	admin.DELETE("/event/:eventId", cancelEvent)

	sameRegisteredUser.GET("/:userId", getUser)
	sameRegisteredUser.PUT("/:userId", updateUser)
	sameRegisteredUser.DELETE("/:userId", disableUser)

	registered.POST("/buy/:seatId", buyTicket)

	public.GET("/place/:placeId", getPlace)
	public.GET("/events", listEvents)
	public.GET("/event/:eventId", getEvent)
	public.POST("/users", createUser) // TODO Checar, me huele raro......
	public.POST("/login", loginUser)
}
```

### How to serve static files in root page?
```go
api:=r.Group("/api")
api.GET("/json", serveJSON)

r.Static("/","./public")
```

Will get the following error:
```
[GIN-debug] GET    /api/json                 --> main.main.func1 (3 handlers)
[GIN-debug] GET    /*filepath                --> github.com/gin-gonic/gin.(*RouterGroup).createStaticHandler.func1 (3 handlers)
panic: wildcard segment '*filepath' conflicts with existing children in path '/*filepath'
```

Solution: Use `github.com/gin-contrib/static` to serve the static file:
```go
func main() {
    r:=gin.Default()
    api:=r.Group("/api")
    api.GET("/json", serveJSON)
    
    //r.Static("/","") // cause errors
    
    // fix: use "static" package to serve the file 
    r.Use(static.Serve("/",static.LocalFile("./front/svlt/public",true)))
    
    r.Run(":5000")
}
```

## How to send contents to client?

### Render HTML Template
```go
func main() {
// Set the router as the default one provided by Gin
router = gin.Default()

// Process the templates at the start so that they don't have to be loaded
// from the disk again. This makes serving HTML pages very fast.
router.LoadHTMLGlob("templates/*")

// Define the route for the index page and display the index.html template
// To start with, we'll use an inline route handler. Later on, we'll create
// standalone functions that will be used as route handlers.
router.GET("/", func(c *gin.Context) {
    // Call the HTML method of the Context to render a template
    c.HTML(
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

// Start serving the application
router.Run()
}
```

### Render Multiple Templates
See [here](https://gist.github.com/anhtran/9150054167b20ec62ccc)

### Sending JSON
```go
c.JSON(http.StatusOK, gin.H{"message": "pong"})
```

### Sending String
```go
c.String(http.StatusOK, "Hello %s", name)
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
today := time.Now().Format("2006-01-02")
theDate = ctx.DefaultQuery("date", today) // http://example.org/timesheet?date="2021-02-01"

// shortcut for ctx.Request.URL.Query().Get("lastname")
lastname := ctx.Query("lastname")
```

// map: `http://localhost:8080/welcome?names[first]=Kevin&names[second]=Andrew`
```go
names := ctx.QueryMap("names")
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