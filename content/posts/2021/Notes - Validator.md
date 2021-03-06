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

### Translation
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
	enTranslator, _ := universal.GetTranslator("en_CA")

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

