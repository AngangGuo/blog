---
title: "Notes   Egnyte"
date: 2021-04-30T15:39:16-07:00
categories:
- Tech
- API
tags:
- Egnyte
- Work
draft: false
---

Our system uploaded the exported Inventory All Fields file into [Egnyte Plateform](https://www.egnyte.com) every day for each facility.
I need to analyze this file to create two reports each week. (Each file is about 70M or more).

By using my regular Egnyte account, I can login to the website, find the file and download it into my computer.
Then import the data into Sqlite database, run some query commands to get the data.
Copy the data into Excel file, format it and send email to our manager.

It would be great if I can do all these automatically using a script. 
Fortunately Egnyte provides some APIs that I can use to access the files programmatically.

## Egnyte Developer Account
### Pre-requirement:
You need to get a regular Egnyte account before you register a developer account.

### Register
* Register a developer account from [here](https://developers.egnyte.com/member/register)
* Egnyte API Support team will approve your API key request through your email

**Note:** 
* Email: Use the email related/linked to your Egnyte domain account

### Get API Key
* Login to [Egnyte developer website](https://developers.egnyte.com/login)
* Click [Get API Key](https://developers.egnyte.com/apps/mykeys)
* From the page you can see the Application name, Key, Secret, Status, User Rate Limits information

### Get Internal Application Token
See [here](https://developers.egnyte.com/docs/read/Public_API_Authentication#Internal-Applications) for details.

For Internal Application, send the authentication request to `{yourdomail}/puboauth/token`.
![Send the Authentication Request](/images/2021/egnyte-token.PNG)

**Note:**
* Please save this access token and use it when making API requests for this user. 
* This token does not expire until the user's password is changed.

### How to get file link?
See [File System API](https://developers.egnyte.com/docs/read/File_System_Management_API_Documentation)

**URL ENCODING Note:**

The Path parameter in the API endpoints described below must be URL encoded in a special way. 
Each element of the path needs to be separately encoded. I.e. the forward slashes ('/') in the path must not be encoded, 
but the text in between must be. 
This is required because filesystem paths on Egnyte can contain characters which are not permitted in the path portion of a URL.

For example, this path: Shared/example?path/$file.txt should be encoded as Shared/example%3Fpath/%24file.txt

```go
// getEgnyteFileLink return the link to the latest Vancouver All Fields file in Egnyte.
// file name: RL Inventory All Fields_04052021_Vancouver, BC (RL).csv
// full path: /Shared/Operations-RL/Daily RL All Fields Reports/RL Inventory All Fields_04052021_Vancouver, BC (RL).csv
func getEgnyteFileLink() string {
	yesterday := toDDMMYYYY(-1)
	return "https://cloudblue.egnyte.com/pubapi/v1/fs-content/Shared/Operations-RL/Daily%20RL%20All%20Fields%20Reports/" +
		"RL%20Inventory%20All%20Fields_" + yesterday + "_Vancouver%2C%20BC%20(RL).csv"
}

// toDDMMYYYY() return date string ddmmyyyy relative to today
// Today: day = 0; Yesterday: day = -1
func toDDMMYYYY(day int) string {
	t := time.Now()
	t.AddDate(0, 0, day)
	return t.Format("02012006")
}
```

### How to download Egnyte file using Go?
```go

// Get token from Env
const Token="69yxxx..."
func downloadCSVTo(fileURL, filename string) error {
	req, err := http.NewRequest("GET", fileURL, nil)
	if err != nil {
		return err
	}
	req.Header.Add("Authorization", "Bearer "+Token)
	client := &http.Client{}
	response, err := client.Do(req)

	defer response.Body.Close()

	if response.StatusCode != 200 {
		return fmt.Errorf("wrong status code: %v", response.StatusCode)
	}

	out, err := os.Create(filename)
	defer out.Close()

	n, err := io.Copy(out, response.Body)
	if err != nil {
		return err
	}
	fmt.Println(fmt.Sprintf("%s: %v bytes downloaded.", filename, n))
	return nil
}
```

