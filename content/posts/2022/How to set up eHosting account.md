---
title: "How to set up eHosting account?"
date: 2022-09-25T09:56:19-07:00
categories:
  - Tech
  - Email
tags:
  - eHosting
  - Outlook
  - Thunderbird
draft: false
---

## eHosting Mailbox

### Creating an eHosting Account
You can create IMAP and POP3 Mailboxes in eHosting control panel.

* Login to https://ehosting.ca/service/
* Email Management > Mailboxes > Create a new mailbox
    * Email Address: andrew@agcfca.net
    * Password: xxx
    * Name of User(optional): Andrew
    * Create

You can also reset mailbox password or delete an account

### Setting `Forward Email`
You can create a dedicated Email for an event(eg. conf2022@agcfca.net) and 
forward all the emails of this account to a general email account.

To create a new forward:
* Email Address: conf2022@agcfca.net
* Destination: info@agcfca.net (can add more here)

### eHosting Mail Server Settings
This is My eHosting Mail Server settings:
```
Incoming Mail Server:	mail1.ehosting.ca
Outgoing Mail Server:	mail1.ehosting.ca
```
You can find your mail server settings from the right side panel of [Hosting Panel](https://ehosting.ca/service/)

See [full list](https://ehosting.ca/customerService/settings.php?settings=mail) for more details. 
Here are some of the settings:

* POP3 Server
```
mail.mecca.ca / port 110
mail.ehosting.ca / port 110
  -> ssl.ehosting.ca  / port 995 (SSL)
mail1.ehosting.ca / port 110
  -> ssl1.ehosting.ca  / port 995 (SSL)
```
* IMAP Server
```
mail.mecca.ca / port 143
mail.ehosting.ca / port 143
  -> ssl.ehosting.ca  / port 993 (SSL)
mail1.ehosting.ca / port 143
  -> ssl1.ehosting.ca  / port 993 (SSL)
```
* SMTP Server
```
Your ISP's SMTP server (recommended)
or  
mail.mecca.ca* / port 25
mail.ehosting.ca* / port 25 or 587**
  -> ssl.ehosting.ca  / port 465 (SSL)***
mail1.ehosting.ca* / port 25 or 587**
  -> ssl1.ehosting.ca  / port 465 (SSL)***

* requires SMTP authentication.
** Some ISPs such as Sprint and Sympatico block their clients from using port 25. (This is my case)
Please switch to use port 587 if you plan to use our SMTP servers to send out emails. 
However you may want to use their provided SMTP servers to send out your emails instead.
```


### eHosting Webmail
Go to website https://ehosting.ca/horde/imp/ or http://mail.ehosting.ca/

## Outlook Settings
### Adding eHosting account
* File > Add Account
  * andrew@agcfca.net
  * Advanced Options > Let me set up my account manually
* Connect
* Select `IMAP`(prefer, or `POP3`)
* Incoming mail > 
  * Server: `mail1.ehosting.ca`; 
  * Port: 143
  * Encryption method: None
* Outgoing mail > 
  * Server: `mail1.ehosting.ca`; 
  * Port: 587(Port 25 doesn't work in my case!!)
  * Encryption method: Auto (requires SMTP authentication)
* Password: xxx
* Connect
* Done

See [eHosting mail settings](https://ehosting.ca/customerService/settings.php?settings=mail) for details

### Add Gmail / Hotmail account
* File > Add Account
  * angang.guo@hotmail.com
  * Connect 

Note: For Gmail, Hotmail, etc. Outlook can set up them automatically.

## Mozilla Thunderbird Settings
Here is the settings if using Mozilla Thunderbird:

### SMTP Server Settings
* User Name: `andrew@agcfca.net`
* Server Name: `mail1.ehostings.ca`
* Port: `465`
* Connection Security: `SSL/TLS`
* Authentication Method: `Normal Password`

### IMAP Server Settings
* User Name: `andrew@agcfca.net`
* Server Name: `mail1.ehosting.ca`
* Port: `993`
* Connection Security: `SSL/TLS`
* Authentication Method: `Normal Password`


## Troubleshooting

### Can't connect to SMTP server
When set the `Outgoing mail` to `mail1.ehosting.ca` and Port: `25`, Outlook show this error message:
```
We couldn't connect to the outgoing SMTP server. 
None of the authentication methods supported by Outlook are supported by your server
```

Solution: Change the port number to `587` solve the problem and select the authentication method(select `Auto` if you are not sure).
```
SMTP - port 25 / 587
POP3 - port 110
IMAP - port 143
Secure SMTP (SSMTP) - port 465
IMAP4 over SSL (IMAPS) - port 993
Secure POP3 (SSL-POP) - port 995
```
