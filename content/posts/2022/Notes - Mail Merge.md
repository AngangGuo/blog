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

### Prepare Email List
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

### Set Outlook Account
