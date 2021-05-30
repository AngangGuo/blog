---
title: "Notes - Palywright-Go"
date: 2021-01-25T17:09:37-08:00
draft: true
---


## Selector

See [Element Selectors](https://playwright.dev/docs/selectors/)

### Number Only
Class identifiers are allowed to start with a number, but ID identifiers are not.
You can't use an ID selector starting as a number: `'#123' is not a valid selector.`

You can use escape the number as `` `#\31 23` `` or `` `id=123` ``

## Common Commands
```go
// launch with options
pw.Chromium.Launch(playwright.BrowserTypeLaunchOptions{
	Headless: playwright.Bool(true),
})
	
// context with options
context, err := browser.NewContext(playwright.BrowserNewContextOptions{
    Locale: playwright.String("en-US"),
    Geolocation: &playwright.BrowserNewContextOptionsGeolocation{
        Longitude: playwright.Float(12.492507),
        Latitude:  playwright.Float(41.889938),
    },
    Permissions:       []string{"geolocation"},
    Viewport:          device.Viewport,
    UserAgent:         playwright.String(device.UserAgent),
    DeviceScaleFactor: playwright.Float(device.DeviceScaleFactor),
    IsMobile:          playwright.Bool(device.IsMobile),
    HasTouch:          playwright.Bool(device.HasTouch),
})
		
// new page with option
page, err := browser.NewPage(playwright.BrowserNewContextOptions{
	RecordVideo: &playwright.BrowserNewContextOptionsRecordVideo{
    	Dir: playwright.String("videos/"),
	},
})
	
// go to page with option
page.Goto("http://whatsmyuseragent.org/", playwright.PageGotoOptions{
	WaitUntil: playwright.WaitUntilStateNetworkidle,
})
	
// screen shot
page.Screenshot(playwright.PageScreenshotOptions{
	Path: playwright.String("foo.png"),
})

// save as pdf
page.PDF(playwright.PagePdfOptions{
	Path: playwright.String("playwright-example.pdf"),
})
				
// click
page.Click("text=download")

// type
page.Type(loginInputUserNameID, iq.userName)

// Eval
page.EvalOnSelectorAll("ul.todo-list > li", "el => el.length")
```

## HTML Select Element
### Show value of a Select element
```go
value,err:=page.EvalOnSelector(selectElementID, "e => e.value")
```

### Set an option value
You can set the select options according to Label, Value, Index or Elements

```go
// by label
v,err:=page.SelectOption("#ctl32_ctl04_ctl03_ddValue",playwright.SelectOptionValues{Labels: playwright.StringSlice("Canada")})

// by index (int); 0 based?
v,err=page.SelectOption("#ctl32_ctl04_ctl15_ddValue",playwright.SelectOptionValues{Indexes: playwright.IntSlice(1)})

// by value (string)
v,err=page.SelectOption("#ctl32_ctl04_ctl15_ddValue",playwright.SelectOptionValues{Values: playwright.StringSlice("2")})
```

Note: You can't select the text with `&nbsp;` space in the label for now. 
See [`&nbsp;` Space Problem](https://github.com/mxschmitt/playwright-go/issues/131)

## iFrame
See [Example](https://github.com/mxschmitt/playwright-go/issues/97)

```go
    frameElement, err := page.QuerySelector("#myframe")
	if err != nil {
		log.Fatalf("could not find #myframe iframe: %v\n", err)
	}
	
	frame, err := frameElement.ContentFrame()
	if err != nil {
		log.Fatalf("could not get content frame: %v\n", err)
	}
	
	fmt.Println(frame.URL())
	fmt.Println(frame.InnerHTML("body"))
```


### Headless Mode
Browser will be lunched in headless mode by default.

To run in non-headless mode:
```go
browser, err := pw.Chromium.Launch(playwright.BrowserTypeLaunchOptions{
  Headless: playwright.Bool(false),
})
```

## HTTP authentication example?
Use `browser.NewContext()` for HTTP authentication

```go
// ignore all error handling
func main() {
	pw, _ := playwright.Run()
	browser, _ := pw.Chromium.Launch(playwright.BrowserTypeLaunchOptions{
		Headless: playwright.Bool(false),
	})
	
	c, _ := browser.NewContext(playwright.BrowserNewContextOptions{
		HttpCredentials: &playwright.BrowserNewContextOptionsHttpCredentials{
			Username: playwright.String(`CORPORATE\my-windows-id`),
			Password: playwright.String("my-windows-password"),
		},
	})

	page, _ := c.NewPage()
	if _, err = page.Goto("http://blueiqreports.na.cloudblue.com/BlueIQ/Pages/Report.aspx?ItemPath=%2fOperations%2fUser+Metrics&ViewMode=Detail"); err != nil {
		log.Fatalf("could not goto: %v", err)
	}

	fmt.Println("success")

	browser.Close()
	pw.Stop()
}

```
