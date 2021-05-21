---
title: "Notes - Palywright-Go"
date: 2021-01-25T17:09:37-08:00
draft: true
---

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