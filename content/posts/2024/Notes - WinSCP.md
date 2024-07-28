---
title: "Notes - WinSCP"
date: 2024-07-18T09:24:23-07:00
categories:
  - Tech
  - Tools
tags:
  - WinSCP
draft: false
---

### Why .csv file is opened in editor?
The default action for double-click a file in server is to open it.

If you double-click a .csv file on the server, it'll be opened in editor instead of downloading.

To download a file from server to local drive, select the file and press F5, or right-click it and select `Download...` 

### How to get the saved password for a session?
* Download `winscppasswd` from [anoopengineer/winscppasswd](https://github.com/anoopengineer/winscppasswd/releases/tag/1.0)
* Open your Windows Registry (`Win + R` and entering `regedit`)
* Navigate to `[HKEY_CURRENT_USER\Software\Martin Prikryl\WinSCP 2\Sessions]`
* Get the Hostname, Username and the encrypted password
* Run `winscppasswd.exe <host> <username> <encrypted_password>`

```
winscppasswd.exe 164.240.2x.1x svr-rl-ca-ops A35C705C2F2A3F***
```

