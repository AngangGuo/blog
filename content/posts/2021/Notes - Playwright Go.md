---
title: "Notes - Playwright-Go"
date: 2021-01-25T17:09:37-08:00
categories:
  - Tech
  - RPA
tags:
  - Go
  - Playwright
draft: false
---

## Best Practices
### Use locators
Locators come with auto waiting and retry-ability. 
Auto waiting means that Playwright performs a range of actionability checks on the elements, 
such as ensuring the element is visible and enabled before it performs the click. 
To make tests resilient, we recommend prioritizing user-facing attributes and explicit contracts.
```
page.getByRole('button', { name: 'submit' });
```

#### Use `codegen` to generate locators
To pick a locator run the codegen command followed by the URL that you would like to pick a locator from.
```
npx playwright codegen your-url(playwright.dev)
```

## Basic Processing
### Read
```
// Read data from label, span, div, etc.
page.Locator(idInventoryDetailsTotalRecords).InnerText()

// input or textarea
page.Locator(idInventoryDetailsLocation).InputValue()

// other - JS code
await page.waitForSelector("#foo")
await page.$eval("#foo", el => el.value)

// using value property of HTMLDataElement interface
await page.locator(inputSelector).evaluate((el) => el.value);

// using innerText property of HTMLElement interface
await page.locator("#element").evaluate(node => node.innerText)
```
### Write
```
// input
page.Locator(idInventoryDetailsLocation).Fill(location)
```

### Other
```
// Click
page.Locator(idInventoryDetailsSearch).Click()

// Goto
page.Goto(blueIQURL, playwright.PageGotoOptions{
		WaitUntil: playwright.WaitUntilStateNetworkidle,
	})
	
// Wait
page.Locator(loginErrorMsgID).WaitFor(
    playwright.LocatorWaitForOptions{
        Timeout: playwright.Float(500),
    })

```


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
For `page.Goto()` function using *WaitUntilState options for fine adjustment.
```
// Other options: WaitUntilStateDomcontentloaded / WaitUntilStateLoad
_, err := iq.page.Goto(blueIQURL, playwright.PageGotoOptions{
    WaitUntil: playwright.WaitUntilStateNetworkidle,
})
```
Or use `WaitForLoadState` function if you already in the page.
```
// LoadStateLoad - not ready, can't get threshold remaining
// LoadStateDomcontentloaded - page doesn't show out but you can get the data - faster
// LoadStateNetworkidle - page is visible and ready - stable but slower
err = iq.page.WaitForLoadState(playwright.PageWaitForLoadStateOptions{State: playwright.LoadStateNetworkidle})

// Or use Locator.WaitFor
err = iq.page.Locator(auditRepairThresholdRemainingID).WaitFor()
```

Note: This function will not work if your page is re-directing to other pages,
like searching an asset tag in BlueIQ and re-directing to repair page.
Use the above `WaitForURL` function instead in this case.

## Selector

See [Element Selectors](https://playwright.dev/docs/selectors/)

## CSS Selector
### Other Selector
Playwright adds custom pseudo-classes like :visible, :has-text(), :has(), :is(), :nth-match() and more.
See [Other locators](https://playwright.dev/docs/other-locators)

```
// Clicks a <button> that has either a "Log in" or "Sign in" text.
await page.locator('button:has-text("Log in"), button:has-text("Sign in")').click();
```

### Table selector
Note: 
* Table row and column are all zero(0) based for Nth() function

```
table := page.Locator("#DXMainTable") // ID selector
table.Locator("tbody > tr").Last().InnerText()
table.Locator("tbody > tr.dxgvDataRow").Count() // Class selector

firstRow := page.Locator("#DXDataRow0")
firstRow.Locator("td").Nth(i).InnerText()
firstRow.Locator("td").Last().InnerText()

// Not pseudo classes with multiple selectors
// not(:required, .item__cost, [disabled], [readonly], [type="date"]) 
// See https://developer.mozilla.org/en-US/docs/Web/CSS/:not
page.Locator("#DXMainTable > tbody > tr:not(#HeadersRow, #FilterRow)").Count()

// Nested selecotr
table.Locator("tbody > tr").Last().Locator("td").Count()
```

### ID Selector
`page.locator('id=my-button')`

### Class Name Selector
`page.locator('.submit-button')`

### Text Selector
`page.locator('css=button#id')`

### XPath Selector
`page.locator('xpath=//button[text()="Submit"]'`

### Select element by label
Excerpt of page source
```
    <tr>
        <td>
            <input id="divDropDown" type="checkbox">
            <label for="divDropDown">Vancouver,&nbsp;BC&nbsp;(RL)</label>
        </td>
    </tr>
```
```
// See https://playwright.dev/docs/selectors#text-selector
page.Click("text=Vancouver") // ok
page.Click("text=Vancouver, BC (RL)") // ok
```
```
// not work
err = page.Click("text=Vancouver,&nbsp;BC&nbsp;(RL)") 
```

### XPath

```
// div text is 'Employee Name'
page.WaitForSelector("//div[text()='Employee Name']")

// parent of the div element
page.WaitForSelector("//div[text()='Employee Name']/..")

```

### Number Only
Class identifiers are allowed to start with a number, but ID identifiers are not.
You can't use an ID selector starting as a number: `'#123' is not a valid selector.`

You can use escape the number as `` `#\31 23` `` or `` `id=123` ``

### `WaitFor` vs `WaitForLoadState`
`WaitFor` is for element
```
err := page.Locator("//input[@id='lastName-input']").WaitFor(playwright.PageWaitForSelectorOptions{
	State: playwright.WaitForSelectorStateVisible,
})

// Default
err := page.Locator("//input[@id='lastName-input']").WaitFor()
```

## Common Commands
```
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
```
// prefer to use WaitForSelector rather than time.Sleep
page.WaitForSelector("//div[text()='Employee Name']")
```

### Get the source HTML
```
// whole page html
html, err := page.Content()

// all product HTML nodes on the page
productHTMLElements, err := page.Locator(".product").All()

// where to store the scraped data
var products []Product
    
// get each product information
for _, productElement := range productHTMLElements {
    name, err := productElement.Locator("h4").First().TextContent()
    ...
    product := Product{}
    product.name = strings.TrimSpace(name)
    products = append(products, product)
}

// export data as .csv
// open the CSV file 
file, err := os.Create("products.csv")
if err != nil {
    log.Fatal("Could not open the CSV output file:", err)
}
defer file.Close()

// initialize a CSV file writer
writer := csv.NewWriter(file)

// define the CSV header row
// and write it to the file
headers := []string{
    "name",
}
writer.Write(headers)

// add each product to the CSV output file
for _, product := range products {
    // convert a Product to an array of strings
    record := []string{
        product.name,
    }

    // write a new CSV record
    writer.Write(record)
}
defer writer.Flush()
```

### Get option value of a Select element
```
// HTML Tag
// <option value="10" selected="1">Oct</option>

// get the option value
value,err := page.Locator("#month").Evaluate("e=>e.value", "") // 10

// also this works
value,err := page.Locator("#month").InputValue() // 10

// get the option text
label,err := page.Locator("#month").Evaluate("e=>e.selectedOptions[0].text", "") // Oct

// Note: InnerText() will return all the option texts of a select
// page.Locator("#month").InnerText() 
//Jan
//Feb
//Mar
//Apr
//...
```

### Set an option value
You can set the select options according to Label, Value, Index or Elements

```
    // by label
    v,err:=page.SelectOption("#ctl32_ctl04_ctl03_ddValue",playwright.SelectOptionValues{Labels: playwright.StringSlice("Canada")})
    
    // by index (int); 0 based?
    v,err=page.SelectOption("#ctl32_ctl04_ctl15_ddValue",playwright.SelectOptionValues{Indexes: playwright.IntSlice(1)})
    
    // by value (string)
    v,err=page.SelectOption("#ctl32_ctl04_ctl15_ddValue",playwright.SelectOptionValues{Values: playwright.StringSlice("2")})
```

Note: You can't select the text with `&nbsp;` space in the label for now. 
See [`&nbsp;` Space Problem](https://github.com/mxschmitt/playwright-go/issues/131)

### How to scroll down the page
Keep in mind that Playwright doesn't offer a built-in method to scroll down the page. 
So, you need to define custom JavaScript logic as in the snippet below. 
```
scrollingScript := `
// scroll down the page 10 times
const scrolls = 10
let scrollCount = 0

// scroll down and then wait for 0.5s
const scrollInterval = setInterval(() => {
    window.scrollTo(0, document.body.scrollHeight)
    scrollCount++

    if (scrollCount === scrolls) {
    clearInterval(scrollInterval)
    }
}, 500)
`
// execute the custom JavaScript script on the page
_, err = page.Evaluate(scrollingScript, []interface{}{})
if err != nil {
        log.Fatal("Could not perform the JS scrolling logic:", err)
}
```

### iFrame
See [Example](https://github.com/mxschmitt/playwright-go/issues/97)

```
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

```
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
```
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

### How to listen for console event?
See [here](https://github.com/mxschmitt/playwright-go/issues/186)
```
messages := make(chan playwright.ConsoleMessage, 1) 
page.On("console", func(message playwright.ConsoleMessage) { 
    messages <- message 
}) 
```

## Installation, Set up, and Configuration
### Install Dependencies
```
// Command line
go run github.com/playwright-community/playwright-go/cmd/playwright@latest install --with-deps
# Or
go install github.com/playwright-community/playwright-go/cmd/playwright@latest
playwright install --with-deps

// Or from inside the program
err := playwright.Install()

// with options
playwright.Install(&playwright.RunOptions{
    Browsers: []string{"chromium"},
    // DriverDirectory:     "/tmp/playwright",
    SkipInstallBrowsers: false,
    Verbose:             true,
})

// Install driver only; See https://github.com/playwright-community/playwright-go/issues/478
driver, _ := playwright.NewDriver(playwright.RunOptions{ SkipInstallBrowsers: true })
err := driver.Install()
```

### HTTP authentication example?
Use `browser.NewContext()` for HTTP authentication

```
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

### Headless Mode
Browser will be lunched in headless mode by default.

To run in non-headless mode:
```
browser, err := pw.Chromium.Launch(playwright.BrowserTypeLaunchOptions{
  Headless: playwright.Bool(false),
})
```

## Avoid Anti-bots
To avoid the simple anti-bots, you can randomize your requests.
The idea is to use proxies to change the output IP and set real User Agent header values.

### Customer User Agent

```
// create a new browser context with a custom user agent 
customUserAgent := "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/123.0.0.0 Safari/537.36"
context, err := browser.NewContext(
    playwright.BrowserNewContextOptions{
        UserAgent: &customUserAgent,
    },
)
```

### HTTP Proxy
To configure an HTTP proxy in Playwright, you have to pass a Proxy object to the Launch() method.

```
// create a Proxy object with the proxy connection URL
proxy := playwright.Proxy {
    Server: "http://211.32.24.28:9083",
}

// initialize a Chromium instance with the specified proxy
browser, err := pw.Chromium.Launch(
    playwright.BrowserTypeLaunchOptions{
        Proxy: &proxy,
        // other configs...
    },
)
```

### Still Blocked
You can try [zenrows](https://www.zenrows.com/blog/playwright-golang#avoid-getting-blocked).