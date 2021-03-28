---
title: "How To Copy Files Between Google Drive?"
date: 2021-03-27T22:48:40-07:00
categories:
- Tech
- Tools
tags:
- Google Drive
draft: false
---

## How to copy files between Google Drive?
* Click the link to go to the shared Google Drive page in browser
* Sign in to your Google Drive account
* Right-click the shared folder and select `Add Shortcut to Drive` to add a shared folder shortcut to a specific folder(`My Drive/XingFu`) of your Drive. You can delete this afterwards.
  
![Create Google Drive Shortcuts](/images/2021/google-drive-shortcut.PNG)
  
* Open `https://colab.research.google.com/`
* Create new Notebook
* Mount your Drive:
```
from google.colab import drive
drive.mount('/gdrive')
```
* After you run the above code, system will show the following message:
```
Go to this URL in a browser: https://accounts.google.com/o/oauth2/auth?client...

Enter your authorization code:
```
* Click the link and login to your Google Drive account to get the authorization code and paste into the box to mount the Drive.
* Goto the shortcut folder
```
%cd /gdrive/MyDrive/XingFu
// verify the folder: /gdrive/My Drive/XingFu
pwd
```
* Copy files by running following commands(create `My Drive/XingFu/Videos` beforehand)
```
!cp -r '见证视频/.' '/gdrive/My Drive/XingFu/Videos'
```

All the videos will be copied to your Drive folder within minutes 


## Reference
* https://colab.research.google.com/notebooks/intro.ipynb
* https://webapps.stackexchange.com/questions/120362/how-to-copy-a-shared-folder-into-my-own-google-drive