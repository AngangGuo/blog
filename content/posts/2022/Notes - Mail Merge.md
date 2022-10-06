---
title: "Notes   Mail Merge"
date: 2022-09-25T09:57:50-07:00
categories:
  - Tech
  - Office
tags:
  - Work
  - Outlook
draft: false
---

See [Use mail merge to send bulk email messages](https://support.microsoft.com/en-us/office/use-mail-merge-to-send-bulk-email-messages-0f123521-20ce-4aa8-8b62-ac211dedefa4)

## Prepare Email List
It's better to use Excel file which easy to edit and add/remove columns.

When using `.csv` file format, there should be at least two columns.
If you only need Email address, you can add a dumb column with `,NA` at the end of each row when edited in IDE,
or add a sequence column(Seq,Email) when edited in Excel.
```
Seq,Email
1,email1@gmail.com
2,email2@gmail.com
3,email3@hotmail.com
```
```
Email,NA
email1@gmail.com,NA
email2@gmail.com,NA
email3@hotmail.com,NA
```

## Setting Up Outlook Account
### Adding Account
* File > Account Info > Add Account >
  * Email address: andrew@agcfca.net
  * Advanced Options: Select `Let me set up my account manually`
* Connect
* Select `IMAP` > IMAP Account Settings
  * Incoming mail:
    * Server: `mail1.ehosting.ca`
    * Port: `143`
  * Outgoing mail:
    * Server: `mail1.ehosting.ca`
    * Port: `587`
    * Encryption method: `Auto`


### Sender Name & Account Name Settings
`Sender name` (also known as `from name`) is the name displayed in the inboxes of your recipients.

The default sender name and account name is your email address. 
It's important to change your sender name to better communicate with others.

It's better to use sender name `AGCF Church` than `agcfca.net.gmail.com`, which makes others thinking it's a junk mail.

Follow these steps to change your IMAP Account Settings(agcfca.net@gmail.com):

File > Account Information > Account Settings > Account Name and Sync Settings
* Your name(Sender name): `AGCF Church`
* Account name(Used inside Outlook): AGCF Media Account
* Reply-to-address: pastor@agcfca.net (option)
* Organization: `AGCF Church`

![Account Name and Sync Settings](/images/2022/outlook-imap-account-settings1.PNG)

### Setting Default Account
Mail Merge function will use the first email account address or the default email address according to the settings.

* File > Account Settings > Email tab > Select an account > Select `Set as default`
* File > Options > Mail > Select `Always use the default account when composing new messages`

![Always use default account](/images/2022/outlook-ehosting-always-default-account.PNG)

## Preparing Word Document
### Creating document
Create a Word document and type in your Email contents(HTML format can have pictures, links, etc.)

### Mail Merge
* Go to `Mailings` > `Start Mail Merge` > `E-mail Messages`
* Go to Mailings > Select Recipients > Use an Existing List ... > Select your mail list file
* (Option) Insert contents from your mail list file into document

## Sending Email
(Option) See above "Setting Up Outlook Account" section if you want to change the Outlook Email account.

In the Word file > Select `Finish & Merge` > `Send Email Messages...` > Merge To Email > Message options
  * To: (Email column)
  * Subject line: `2022 美加西幸福小组研习会 邀请函`
  * Mail format: `HTML`
  * Send records: All | Current record | From..To..
  * OK

![Merge to email](/images/2022/mail-merge-merge-to-email.PNG)
