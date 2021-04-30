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

Our Inventory All Fields files are kept in .
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
