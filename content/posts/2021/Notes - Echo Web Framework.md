---
title: "Notes   Echo Web Framework"
date: 2021-03-23T14:38:32-07:00
draft: true
---

## Echo Web Framework

### How to set up a validator?
```go
type CustomValidator struct {
    Validator *validator.Validate
}

func (c *CustomValidator) Validate(i interface{}) error {
	return c.Validator.Struct(i)
}

// set up the Validator from main.go
e.Validator = &CustomValidator{Validator: validator.New()}
```

### How to bind a checkbox?
```go
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

