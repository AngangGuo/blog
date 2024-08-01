---
title: "Notes - Playwright-Go"
date: 2021-01-25T17:09:37-08:00
categories:
  - Tech
  - Programming 
tags:
  - Go
  - Playwrite
draft: false
---

## Tips
### How to wait for one of several pages to show out?
Use regular expression to check each one of the pages may be showed out.
```
// Repair tab
// auditRepairURL = `https://blueiq.cloudblue.com/Audit/RepairDetailLOB.aspx`

// Test tab
// auditTestURL = `https://blueiq.cloudblue.com/Audit/AssetConfiguration.aspx`

// Not found tab
// auditSummaryURL = "https://blueiq.cloudblue.com/Audit/Summary.aspx"

// Any one of the above three cases
var reAssetURLs = regexp.MustCompile("RepairDetailLOB|AssetConfiguration|Summary")

// It will be done if any one of the above url reached
err = iq.page.WaitForURL(reAssetURLs, playwright.PageWaitForURLOptions{
	WaitUntil: playwright.WaitUntilStateDomcontentloaded,
})

// check which url reached
url := iq.page.URL()
```

### How to get the page ready?
For Goto page function using *WaitUntilState options for fine adjustment.
```
// Other options: WaitUntilStateDomcontentloaded / WaitUntilStateLoad
_, err := iq.page.Goto(blueIQURL, playwright.PageGotoOptions{
    WaitUntil: playwright.WaitUntilStateNetworkidle,
})
```
Or use `WaitForLoadState` function if you already in the page.

Note: This function will not work if your page is re-directing to other pages, 
like searching an asset tag in BlueIQ and re-directing to repair page.
Use the above `WaitForURL` function instead in this case.
```
// LoadStateLoad - not ready, can't get threshold remaining
// LoadStateDomcontentloaded - page doesn't show out but you can get the data - faster
// LoadStateNetworkidle - page is visible and ready - stable but slower
err = iq.page.WaitForLoadState(playwright.PageWaitForLoadStateOptions{State: playwright.LoadStateNetworkidle})

// Or use Locator.WaitFor
err = iq.page.Locator(auditRepairThresholdRemainingID).WaitFor()
```

## Selector

See [Element Selectors](https://playwright.dev/docs/selectors/)

### Select element by label
Excerpt of page source
```
    <tr>
        <td>
            <input id="ctl32_ctl04_ctl07_divDropDown_ctl70" type="checkbox">
            <label for="ctl32_ctl04_ctl07_divDropDown_ctl70">Vancouver,&nbsp;BC&nbsp;(RL)</label>
        </td>
    </tr>
```
```go
// See https://playwright.dev/docs/selectors#text-selector
err = page.Click("text=Vancouver") // ok
err = page.Click("text=Vancouver, BC (RL)") // ok
```
```
// not work
err = page.Click("text=Vancouver,&nbsp;BC&nbsp;(RL)") 
```

### XPath

```javascript
// div text is 'Employee Name'
page.WaitForSelector("//div[text()='Employee Name']")

// parent of the div element
page.WaitForSelector("//div[text()='Employee Name']/..")

```

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
// type with optional delay
page.Type("#ctl32_ctl04_ctl11_txtValue",today, playwright.PageTypeOptions{Delay: playwright.Float(100.0)})

// Eval
page.EvalOnSelectorAll("ul.todo-list > li", "el => el.length")
```

## Select HTML Element
### Wait for selector
```go
// prefer to use WaitForSelector rather than time.Sleep
page.WaitForSelector("//div[text()='Employee Name']")
```

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

### iFrame
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

### Download Files
* All the downloaded files belonging to the browser context are deleted when the browser context is closed.
* Browser context must be created with the `acceptDownloads` set to `true` when user needs access to the downloaded content.

```go
    // 1. Browser instance
    browser, err := pw.Chromium.Launch(playwright.BrowserTypeLaunchOptions{
        Headless: playwright.Bool(false),
        DownloadsPath: playwright.String(`C:\Andrew\prj\rl\learn`),
    })
    
    // 2. Browser context
    c, err := browser.NewContext(playwright.BrowserNewContextOptions{
        HttpCredentials: &playwright.BrowserNewContextOptionsHttpCredentials{
            Username: playwright.String(`CORPORATE\my-win-id`),
            Password: playwright.String("my-password"),
        },
        AcceptDownloads: playwright.Bool(true),
    })
    
    // 3. Download
    download,err:=page.ExpectDownload(func() error {
        return page.Click("#ctl32_ctl05_ctl04_ctl00_Menu > div:nth-child(6) > a")
    })
    
    // 4. Save the file
    fileName := download.SuggestedFilename()
    err = download.SaveAs(path.Join("./", fileName))
```

### Run JavaScript function on selector
```go
// 1. simple example
page.EvalOnSelector("//div[text()='Employee Name']/../../..",`(el) => el.nodeName`) // TBODY

// 2. complex function example
// run function on the selector
f:=`
(el) => {
	let result = "", name = "", client = "", liq = "";
	let sep = "$" // can't use comma, because the comma is used in name
	
	if (el.nodeName==="TBODY"){
		for (let j=1;j<el.rows.length;j++){
		    name = el.rows[j].cells[1].textContent
		    client = el.rows[j].cells[6].textContent
		    liq = el.rows[j].cells[7].textContent
			result = result + name + sep + client + sep + liq + "\n"
		}
	}

	return result
}
`
csvStats, _ := page.EvalOnSelector("//div[text()='Employee Name']/../../..", f)
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

## Event
### How to listen for console event?
See [here](https://github.com/mxschmitt/playwright-go/issues/186)
```go
messages := make(chan playwright.ConsoleMessage, 1) 
page.On("console", func(message playwright.ConsoleMessage) { 
    messages <- message 
}) 
```