---
title: "Notes - Validator"
date: 2021-03-26T13:45:20-07:00
categories:
- Tech
- Web
tags:
- Go
- Validator
draft: false
---

## Validator Usage
[go-playground/validator](https://github.com/go-playground/validator) 

Note: Validator will fail at the first tag that violet the rule. 

For example, a string "A.G" with rule "alpha,min=10" will cause the following error:
```go
type User struct {
	Name string `validate:"required,alpha,min=10,max=15"`
}

func main() {
	v := validator.New()
	a := User{"A.G"}
	err := v.Struct(a)
	fmt.Println(err)
    // error message: 
    // Key: 'User.Name' Error:Field validation for 'Name' failed on the 'alpha' tag
}
```

Removing the dot("AG") and run it again, it will show this error message:
```text
Key: 'User.Name' Error:Field validation for 'Name' failed on the 'min' tag
```

### Validate Variable
```go
v := validator.New()
name := "Andrew"
err = v.Var(name, "required,min=10,max=15")
fmt.Println(err)

```

### Validate Struct
```go
type User struct {
	Name string `validate:"required,min=10,max=25"`
}

func main() {
	v := validator.New()

	u1 := User{
		Name: "Andrew G",
	}
	err := v.Struct(u1)
	fmt.Println(err)
	// Key: 'User.Name' Error:Field validation for 'Name' failed on the 'min' tag
}
```

### Validate Map
```go
validate := validator.New()
user := map[string]interface{}{"name": "Arshiya Kiani", "email": "zytel3301@gmail.com"}

// Every rule will be applied to the item of the data that the offset of rule is pointing to.
// So if you have a field "email": "omitempty,required,email", the validator will apply these
// rules to offset of email in user data
rules := map[string]interface{}{"name": "required,min=8,max=32", "email": "omitempty,required,email"}

errs := validate.ValidateMap(user, rules)

// ValidateMap will return map[string]error.
// The offset of every item in errs is the name of invalid field and the value
// is the message of error. If there was no error, ValidateMap method will
// return an EMPTY map of errors, not nil. If you want to check that
// if there was an error or not, you must check the length of the return value
if len(errs) > 0 {
    fmt.Println(errs)
    // The user is invalid
}

// The user is valid
```

See [map-validation](https://github.com/go-playground/validator/blob/master/_examples/map-validation/main.go)

### Customize Validation
* For customer validate a struct, see example [here](https://github.com/go-playground/validator/blob/master/_examples/struct-level/main.go)
* For customer validate a tag, see example [here](https://github.com/go-playground/validator/blob/master/_examples/custom-validation/main.go)
* For customer validate a type, see example [here](https://github.com/go-playground/validator/blob/master/_examples/custom/main.go)

## Translate
### Translation Example
See [Golang使用validator进行数据校验及自定义翻译器](https://wangyangyangisme.github.io/2020/10/20/golang-Golang%E4%BD%BF%E7%94%A8validator%E8%BF%9B%E8%A1%8C%E6%95%B0%E6%8D%AE%E6%A0%A1%E9%AA%8C%E5%8F%8A%E8%87%AA%E5%AE%9A%E4%B9%89%E7%BF%BB%E8%AF%91%E5%99%A8/)
Also [golang validator参数校验 中文](https://studygolang.com/articles/27728)

https://www.liwenzhou.com/posts/Go/validator_usages/

```go
package main

import (
	"fmt"
	enLocale "github.com/go-playground/locales/en_CA"
	zhLocale "github.com/go-playground/locales/zh"
	uTranslator "github.com/go-playground/universal-translator"
	"github.com/go-playground/validator/v10"
	enTranslation "github.com/go-playground/validator/v10/translations/en"
	zhTranslation "github.com/go-playground/validator/v10/translations/zh"
)

type User struct {
	FirstName string `validate:"required"`
	LastName  string `validate:"required,min=4"`
	Age       int    `validate:"gte=4,lte=60"`
}

func main() {
	en := enLocale.New()
	zh := zhLocale.New()
	// 万能翻译器，保存所有的语言环境和翻译数据
	universal := uTranslator.New(en, zh)

	zhTranslator, _ := universal.GetTranslator("zh")
	enTranslator, _ := universal.GetTranslator("en")

	myValidator := validator.New()
	_ = zhTranslation.RegisterDefaultTranslations(myValidator, zhTranslator)
	_ = enTranslation.RegisterDefaultTranslations(myValidator, enTranslator)

	u := User{
		"Andrew",
		"Guo",
		2,
	}
	err := myValidator.Struct(&u)
	if err != nil {
		fmt.Println(err.(validator.ValidationErrors).Translate(zhTranslator))
		fmt.Println("--")
		for _, e := range err.(validator.ValidationErrors) {
			fmt.Println(e.Translate(zhTranslator))
			fmt.Println(e.Translate(enTranslator))
		}
	}
}

Output:
==================
map[User.Age:Age必须大于或等于4 User.LastName:LastName长度必须至少为4个字符]
--
LastName长度必须至少为4个字符
LastName must be at least 4 characters in length
Age必须大于或等于4
Age must be 4 or greater
```

```go
// 初始化翻译器
func validateInit() {
    zh_ch := zh.New()
    uni := ut.New(zh_ch)               
    Trans, _ = uni.GetTranslator("zh") // 翻译器
    Validate = validator.New()
    _ = zh_translations.RegisterDefaultTranslations(Validate, Trans)
    // 添加额外翻译
    _ = Validate.RegisterTranslation("required_without", Trans, func(ut ut.Translator) error {
        return ut.Add("required_without", "{0} 为必填字段!", true)
    }, func(ut ut.Translator, fe validator.FieldError) string {
        t, _ := ut.T("required_without", fe.Field())
        return t
    })
}
```

### Translate Field Name
By default, the field name is not translated. Use `RegisterTagNameFunc` to get the field name from a specific tag.

```go
// add tag for the field name(cn in this case, it can be any tag, like json tag)
type User struct {
	Name string `validate:"required,min=1,max=15" cn:"姓名"`
}

v:=validator.New()
v.RegisterTagNameFunc(func(field reflect.StructField) string {
    label := field.Tag.Get("cn")
    if label == "" {
        return field.Name
    }
    return label
})

```
See [example 1](https://www.gushiciku.cn/pl/pHrV/zh-tw) and 
[example 2](https://github.com/go-playground/validator/blob/master/_examples/struct-level/main.go)

## Validator & Echo web framework
### How to use validator in Echo?

```go
// 1. Implement the interface
type CustomValidator struct {
    validator *validator.Validate
}

func (cv *CustomValidator) Validate(i interface{}) error {
  return cv.validator.Struct(i)
}

// 2. Register the method
e.Validator = &CustomValidator{validator: validator.New()}

// 3. Add validate tags in your struct
type User struct {
    Name  string `json:"name" validate:"required"`
    Email string `json:"email" validate:"required,email"`
}

// 4. Validate the struct from your handler
u := new(User)
if err := c.Bind(u); err != nil { ... }
if err := c.Validate(u); err != nil {...}
```

### How to validate a path parameter?
```go
id := c.Param("id")
// id type is string, gt and lt is lenth of the string
err = Validate.Var(id, "required,number,gt=1,lt=5") 
```
Prefer the following:
```go
var id int
err:= echo.PathParamsBinder(c).Int("id",&id).BindError()
// id type is int
err = Validate.Var(id, "gt=0,lte=9999")
```

### How can I validate a variable from within Echo?
After you register validator in Echo, you can only validate struct by using the Context.Validate().

To validate a variable, wrap it inside a struct. You can use anonymous struct like this:
```go
a := struct {
  ID int `param:"id" validate:"required,gt=0,lt=99999"`
}{}
err := c.Bind(&a)
err = c.Validate(a)
```
Alternatively, you can create a validator instance and use it normally:
```go
// create a new instance
var Validate = validator.New()

// use it in your handler
id := c.Param("id")
err := Validate.Var(id, "required,gt=0,lte=99999")
```

## Useful Links
https://medium.com/tunaiku-tech/go-validator-v10-c7a4f1be37df
