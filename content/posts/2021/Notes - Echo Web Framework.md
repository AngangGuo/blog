---
title: "Notes - Echo Web Framework"
date: 2021-03-23T14:38:32-07:00
categories:
- Tech
- Web
tags:
- Go
- Echo
draft: false
---

## Echo Web Framework

### How to get URL parameter and query parameter values?
```
// localhost/user/123?name=Andrew&age=12
e.GET("/user/:id", showInfo)

func showInfo(c echo.Context) error {
    // name, age are get from url parameters; 
    myname := c.QueryParam("name") // Andrew
    myage := c.QueryParam("age") // 12
    
    // id is get from url path
    myid := c.Param("id") // 123
	...
}	
```

## Binding
### Binding request data
In the struct definitions each field can be tagged to restrict binding to specific source.
```
    query - source is request query parameters.
    param - source is route path parameter.
    header - source is header parameter.
    form - source is form. Values are taken from query and request body. Uses Go standard library form parsing.
    json - source is request body. Uses Go json package for unmarshalling.
    xml - source is request body. Uses Go xml package for unmarshalling.
```

Note:
* For query, param, header, form only fields with tags are bound.
* Each step can overwrite bound fields from the previous step. 

This means if your json request has query param &name=query and body is {"name": "body"} then the result will be User{Name: "body"}.

Request data is bound to the struct in given order:
1. Path parameters
2. Query parameters (only for GET/DELETE methods)
3. Request body

### How to Bind path parameters instead of JSON?
If you have the same parameter in URL and JSON data in the request body, they JSON data will overwrite the path parameter.
If you want to keep the path parameter, you can use these methods:
```
// use the "-" json tag
var params struct {
    Order string `param:"order" json:"-"`
}

// or use BindPathParams method
var params struct {
	Order string `param:"order"`
}
if err := echo.BindPathParams(c, payload); err != nil {
	return err
}
```

Here is a list of Bind methods:
* BindBody
* BindHeaders
* BindPathParams
* BindQueryParams

### How to POST JSON data by using Postman?
* Select "POST" method
* Type in the URL(http://localhost:1323/save)
* Select `Body` > send `raw` data> `JSON` format
* Compose your data {"id": "1"}
* Click `Send`

### How to process JSON request data? 
Note: You can't POST the form data as JSON data. Use Postman shown above to send raw JSON data.

```
type Student struct {
  ID string `json:"id"`
}

func getStudent(c echo.Context) error {
    student := Student{}
    // Method 1
    _ := c.Bind(&student)
    
    // defer c.Request().Body.Close()
    // Method 2
    // _ :=json.NewDecoder(c.Request().Body).Decode(&student)
    
    // Method 3
    // data, _ := io.ReadAll(c.Request().Body)
    // _ = json.Unmarshal(data, &student)
    
    return c.JSON(http.StatusOK, student)
}
```

### How to bind a checkbox?
```
type PostForm struct {
    Name  string
    Agree bool
}
```
If the value attribute was omitted, the default value for the checkbox is `on`;
If a checkbox is unchecked when its form is submitted, 
there is no value submitted to the server to represent its unchecked state.

The default value of checkbox is "on", which is different with go's "true";
Set the value specifically as `true` will be ok.
```html
<input type="checkbox" name="agree" value="true">
```

## Respond
### How to send message continually?

```
e.GET("stream", func(c echo.Context) error {
    c.Response().Header().Set(echo.HeaderContentType, echo.MIMETextPlain)
    // this setting is important
    c.Response().Header().Set("X-Content-Type-Options", "nosniff")
    c.Response().WriteHeader(http.StatusOK)
    for i := 0; i < 4; i++ {
        c.Response().Write([]byte(
            fmt.Sprintf("%s,%v\n", "processing", i)))
        c.Response().Flush()
        time.Sleep(time.Second)
    }

    c.Response().Write([]byte("Done\n"))
    c.Response().Flush()
    return nil
})
```

For streaming JSON response see this [example](https://echo.labstack.com/cookbook/streaming-response/)

## Static Files
### How to serve the embedded static files?
```
// Embed static folder
//go:embed static
var staticEmbed embed.FS
...

// Serving static files
// func (e *Echo) StaticFS(pathPrefix string, filesystem fs.FS) *Route
// func MustSubFS(currentFs fs.FS, fsRoot string) fs.FS
e.StaticFS("/static", echo.MustSubFS(staticEmbed, "static"))

// Using embedded static file(image) in template file
<img src="/static/icon.jpg">
```

## Middleware
### Middleware levels
You can use middleware in three levels:
* Root
* Group
* Route

```
// root level
e.Use(middleware.Logger())

// group level
g := e.Group("/admin")
g.Use(middleware.BasicAuth(func(username, password string, c echo.Context) (bool, error) {
    if username == "joe" && password == "secret" {
    return true, nil
    }
    return false, nil
}))

// route level
// e.Get("/user", getUser, myMiddleWare)
track := func(next echo.HandlerFunc) echo.HandlerFunc {
    return func(c echo.Context) error {
        println("request to /users")
        return next(c)
    }
}
e.GET("/users", func(c echo.Context) error {
    return c.String(http.StatusOK, "/users")
}, track)
```

### How to write a custom middleware?

```
// AddHeader middleware will add header key/value to echo server
func AddHeader(next echo.HandlerFunc) echo.HandlerFunc {
    return func(c echo.Context) error {
        c.Response().Header().Set(echo.HeaderServer, "MyOwnServer")
        // In Chrome, the key is always capitalized: Anykey:anyvalue
        c.Response().Header().Set("anykey", "anyvalue")
        log.Println("middleware executed")
        return next(c)
    }
}
```

**Tips**
To view the request or response HTTP headers in Google Chrome, take the following steps :
* In Chrome, press "Ctrl + Shift + J" to open the developer tools.
* Select Network tab.
* Reload the page
* Select the HTTP request on the left panel, and the HTTP headers will be displayed on the right panel.

## Validator
### How to set up a validator?
See [Validator](/posts/2021/Notes-Validator/) for advanced usage.

```
type CustomValidator struct {
    Validator *validator.Validate
}

func (c *CustomValidator) Validate(i interface{}) error {
    return c.Validator.Struct(i)
}

// set up the Validator from main.go
e.Validator = &CustomValidator{Validator: validator.New()}
```

### Route `/admin` vs `/admin/`
```
admin := e.Group("admin")
// example.com/admin
admin.GET("", func(c echo.Context) error {
    return c.String(200, "admin root")
})
// example.com/admin/
admin.GET("/", func(c echo.Context) error {
    return c.String(200, "in admin")
})
```

You can use [trailing slash middleware](https://echo.labstack.com/middleware/trailing-slash/) to add or remove the trailing slash of a request uri.

### JWT 
```
// import "github.com/golang-jwt/jwt/v4"
// https://pkg.go.dev/github.com/golang-jwt/jwt/v4

func main() {
	secret := []byte("MySecret")

	// Create the Claims
	claims := &jwt.RegisteredClaims{
		ExpiresAt: jwt.NewNumericDate(time.Now().Add(5 * time.Minute)),
		Issuer:    "Andrew",
	}

	token := jwt.NewWithClaims(jwt.SigningMethodHS256, claims)
	ss, err := token.SignedString(secret)
	fmt.Printf("%v %v\n", ss, err)
	//Output: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJBbmRyZXciLCJleHAiOjE2NDI5OTAwMzR9.a2L5y-6XT0-dcmHL5YA2C7zKiA1yT9hKbsgeV0gRxLo
	
	// Parse
	info := jwt.RegisteredClaims{}
	myToken, err := jwt.ParseWithClaims(ss, &info, func(t *jwt.Token) (interface{}, error) {
		return secret, nil
	})
	if err != nil {
		log.Fatalln(err)
	}
	fmt.Println(myToken)
	fmt.Println(info)
	// {Andrew  [] 2022-01-23 18:07:14 -0800 PST <nil> <nil> }
}
```

See Also:
* [jwt.io](https://jwt.io/)
* [Go JWT library](https://github.com/golang-jwt/jwt)
* [real world example](https://github.com/victorsteven/Go-JWT-Postgres-Mysql-Restful-API/blob/master/api/auth/token.go)
* [manage jwt](https://github.com/victorsteven/manage-jwt) See how you can invalidate JSON without waiting for the token to expire

## FAQ
### How to make Echo serve an SPA correctly?
Echo has problems with SPA routing. To make Echo work well with your SPA, you need to enable the `HTML5` property for the Echo static middleware. 
```
e.Use(middleware.StaticWithConfig(echoMw.StaticConfig{
    // This is the path to your SPA build folder, the folder that is created from running "npm build"
    Root:   "public",	
    
    // This is the default html page for your SPA
    Index:  "index.html",	
    
    // Enable HTML5 mode by forwarding all not-found requests to root so that
    // SPA (single-page application) can handle the routing.
    HTML5:  true,
}))
```
