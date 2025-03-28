---
title: "Notes - Miscellaneous"
date: 2021-07-30T08:49:20-07:00
categories:
  - Tech
  - Other
tags:
  - Outlook
  - Markdown
  - Windows
  - Miscellaneous
draft: false
---

## Useful Website Links
* [File Sharing/transfers](filetransfer.io) 
* [Compare list](www.listdiff.com) 
* [Convert a website table to Markdown](https://tabletomarkdown.com/convert-website-table-to-markdown/)
* [Convert Spreadsheet table to Markdown](https://tabletomarkdown.com/convert-spreadsheet-to-markdown/)
* myapps.microsoft.com: my applications

## Microsoft PowerToys
[Microsoft PowerToys](https://learn.microsoft.com/en-us/windows/powertoys/) is a set of utilities for power users to tune and streamline their Windows experience for greater productivity.

[PowerToys Awake](https://learn.microsoft.com/en-us/windows/powertoys/awake) is a tool for Windows designed to keep a computer awake without having to manage its power and sleep settings. 
This can be helpful when running time-consuming tasks, ensuring that the computer does not go to sleep or turn off its screens.

Download the PowerToys from [here](https://github.com/microsoft/PowerToys/releases).
If you don't have admin access, you can download the Per User installer.

### PowerRename
See [Regular Expression](https://en.wikipedia.org/wiki/Regular_expression)

Examples
```
// Change Slide1.JPG to training01.JPG
Search: Slide([\d])
Replace: training0$1
```

## Microsoft Edge Browser
### Edge browser asking for PIN
When I refresh the page and want to access the blueiq report, Edge keeps asking for the PIN.
It won't let me in even if I entered the right pin.

Solution:
Select other account, then type the email and password instead of pin.

## Google Drive
### How to update the file but keep the shared link?
You can upload a file from your computer to Google Drive and share it with others. 
If the original file in your computer has been updated to a new version. 
If you want to update the file in Google Drive to this new version, you can upload it to Google Drive again and select
`Replace existing file` in the `Upload options` window. Replacing the file won't change sharing settings.

## Windows
### How to record your screen on Windows 11?
You can record your screen by using Snipping Tool
* Open Snipping Tool
* In the top toolbar, press the Record button, then press New.
* Click to drag and drop a rectangle indicating the portion of the screen you would like to record.
* Press Start and you will see a 3-second countdown begin for your recording.
* After you finish, press Stop. 
* Save it, or edit it in Microsoft Clipchamp

### How to show file structures?
```
tree /f > tree.txt
// or
dir /A:D /B /S
```

### USB scanner always in low battery status
See [this](https://www.drivereasy.com/knowledge/usb-selective-suspend-everything-you-need-to-know-about-it/)

Frankly speaking, the `USB selective suspend` is really handy for laptop and tablet users in that it saves you power from unnecessary USB devices.
But if you run into USB device not recognized problem, always have the charger plugged in your computer, 
or you are on a full-power required USB device at all times, you should disable this feature.

Go to Power Plan settings > Change plan settings > Change advanced power settings > USB settings >
USB selective suspend setting > `Disabled`

### How to find a string from text files?
```
// Search in book folder
findstr painting "D:\books\*"

// Search in sub-folder as well
findstr /s painting "D:\books\*"

// Ignore the case; with line number; in the .txt files; including subfolder
findstr /s /i /n painting "D:\books\*.txt"
```
```
PS Q:\Manifest Files\> findstr 7131 *.csv
Manifest74d.csv:LPNRRDO5571318,B08888C5HG,201,Home,7007 Handheld Vacuums,40.48,BISSELL International Trading Co,2994B
Manifest74d.csv:LPNRRDO2307131,B08YRV5756,79,Kitchen,5022 Stand Mixers,108.87,,SM-1553N
Manifest465.csv:LPNRREA0771316,B08P2D6RDT,201,Home,7001 Upright Vacuums,141.16,Eureka,NEU100C
```

## Media
## Canon XA25
### audio settings
See [here](https://support.usa.canon.com/kb/index?page=content&id=ART140508)

### How to adjust the microphone sensitivity? (INPUT terminals)
You can adjust the sensitivity of the input of the INPUT 1 and INPUT 2 terminals. 
You can set the sensitivity switch of the desired INPUT terminal to MIC or MIC+48V.

Func > Menu > INPUT 1 Mic Trimming > Select the desired level(The available sensitivity levels are +12 dB, +6 dB, 0 dB, -6 dB and -12 dB.)

### When should I activate the microphone attenuator? (INPUT terminals)
You can activate the microphone attenuator (20 dB) for microphones connected to the INPUT 1 and INPUT 2 terminals to 
prevent audio from being distorted if the audio level is too high.

Set the sensitivity switch of the desired INPUT terminal to MIC or MIC+48V.

Func > Menu > INPUT 1 Mic Att. > On

## PC
### What's the difference between Pink, Blue, and Green for audio ports?
* Green - L/R Line out
* Blue - L/R Line in
* Pink - Mic in

### How to connect a mixer to PC using Line In?
See [Video](https://www.youtube.com/watch?v=RzD0B3LgDvg)

* Use a dual 1/4" TS to 3.5mm TRS cable <br>
Connect the 1/4" to mixer main out ports, 
then connect the 3.5mm to the blue port in PC.

* Use RCA to 3.5mm cable <br>
Connect the RCA cable to the RCA out port in mixer,
then connect the 3.5mm to the blue port in PC

### How to install Xerox printer driver?
Network printer Xerox AltaLink C8155 in ALC office, Windows can't find the driver.

[Xerox Smart Start - Driver Installer](https://www.support.xerox.com/en-ca/product/altalink-c8100-series/content/143617).
intelligently looks at your specific system configuration and installs the appropriate drivers 
for printing and scanning to your Xerox device.

I use it to install the Xerox AltaLink C8155 printers in DC by searching the network.
Select your printer and it'll install the driver automatically.

## MobaXterm
[MobaXterm](https://mobaxterm.mobatek.net/) is an enhanced terminal for Windows with X11 server, tabbed SSH client, 
network tools and much more. It's the free alternative for my [SecureCRT](https://www.vandyke.com/products/securecrt/).

### Why I can't upload the files?
When using SFTP upload file from your local drive, it'll fail if there're Chinese characters in the path.
Move the file to other path with English letters only.
```
Opening directory /var/caddy/xingfu.brideofchrist.ca/static...
Open directory command received
Directory content listed
Starting SFTP transfer
Uploading file "C:\Users\angan\Google Drive\????\??????\????????\Blessed.mp4" to "/var/www/html/static/Blessed.mp4" (0.00 MB)
Error EInOutError: I/O error 123
SFTP error #2: No such file
```

MobaXterm can't show Chinese character in SFTP window. 
Use [FileZilla](https://filezilla-project.org/download.php?show_all=1) instead.

## Teams
### How to quote message in Teams desktop?
Quote using a keyboard shortcut
* Copy the message
* On your keyboard press “Shift” + “>”
* Paste the original message and then type your own message
* Press Enter twice and to type your message after the quote

### How to create a chat group?
* Select the `New Chat` button(or press `Ctrl+N`)
* In the `To:` line, type the email addresses of those you want to include in this group
* Type a message and send it to finish creating your new group
* Click the pencil button at the top to change the group name

See [here](https://support.microsoft.com/en-us/office/create-a-group-chat-556d9323-75f4-4cbe-ba49-e65d7d8d53a8)

## OBS
### How to run OBS in portable mode?
Windows
First, download the ZIP from the [Download page](https://obsproject.com/download) and unzip it.

There are two methods to launch OBS in portable mode:
* Either place an empty `portable_mode` or `portable_mode.txt` file in the root directory and open `obs64.exe` in `bin/64bit/`.
* Or, launch `bin/64bit/obs64.exe` using the `--portable` flag.

Note: You may need to install C++ to make OBS work.

### How to stop steaming automatically?
Tools > Output Timer > Stop streaming after: fill in hours > Start

## Office
### How to add barcode to a Word document?
See [here](https://support.microsoft.com/en-us/office/field-codes-displaybarcode-6d81eade-762d-4b44-ae81-f9d3d9e07be3)
* Click or tap where you want the barcode.
* Press `Ctrl + F9`. You must do this, as just typing curly brackets {} won't work.
* Type `DisplayBarcode` plus arguments and switches. See below table for more details.
* To see the barcode, right-click the Field code and select `Toggle Field Codes`.

| Code type         | DisplayBarcode syntax                              |
| ----------------- |----------------------------------------------------|
| QR code           | DisplayBarcode “http://www.microsoft.com” QR \\q 3 |
| EAN13 (with text) | DisplayBarcode “490123456789” EAN13 \\t            |
| Code 39           | DisplayBarcode “2345678” CODE39 \\d \\t            |
| Code 128          | DisplayBarcode “PI-001” CODE128 \\t                |
| UPCA              | DisplayBarcode "012345678901" UPCA \\t \\x         |
| UPCE              | DisplayBarcode "123456" UPCE \\t \\x               |

### How to view the previous version of Office files?
* Open the file you want to view.
* Click File > Info > Version history. 
* Select a version to open it in a separate window.
* If you want to restore a previous version you've opened, select Restore.

## PowerPoint
### How to export slides as high-resolution JPEG?
By default, the export resolution of a PowerPoint slide that you want to save as a picture is 96 dots per inch (dpi), 
it'll be 1280 x 720 pixels for wide screen.

To change the export resolution, you need to change the settings in Windows registry file.
```
// regedit
HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\PowerPoint\Options
Edit > New > DWORD (32-bit) Value > ExportBitmapResolution > 
one of these values: 50(667 x 375), 96(1280 x 720), 100(1333 x 750), 150(2000 x 1125), 200(2667 x 1500), 250(3333 x 1875)
```

See [here](https://docs.microsoft.com/en-us/office/troubleshoot/powerpoint/change-export-slide-resolution) for details.

## Outlook
### How to convert plain text mail to HTML?
* For Individual Email: 
Click `Replay` > `Pop Out` > `Format Text` > Select `HTML`

* For all Emails: 
`File` > `Option` > `Mail` > `Compose messages in this format` > `HTML`

### How to recall an Email?
Open the email in a separate window >
File > Info > Resend or Recall > Recall this message

### How can I show meetings in highlight color?
For some important meetings, you may want to highlight them so that it can stand out from among your other meetings.

Right-click on the appointment/meeting/event, select `Catagorize` > `Yellow category`(or any other colors you like).
You can name the colored category to reflect your purpose.

### How can I show holidays in my Calendar?
File > Options > Calendar > Calendar options > Add Holidays > Select your locations(Canada)

### How to set your calendar working time?
I start working at 8:00am, but my calendar always show my schedule starting at 8:30. 
I even missed an appointment with our manager at 8:00 because the appointment is hidden at the top line.

You can change your working hours by setting Start time, End time, Work week, etc. from within Outlook:

File > Options > Calendar > Work time > Work hours

### How to recover deleted message/folder in Outlook?
See [doc](https://support.microsoft.com/en-us/office/recover-deleted-items-in-outlook-for-windows-49e81f3c-c8f4-4426-a0b9-c0fd751d48ce)

* You can recover the deleted messages in the `Deleted Items` folder.
* You can find the deleted folder by click the `>` mark in front of the `> Deleted Items` folder.

Then you can select the deleted message/folder and move it out.

If you can't find the deleted item in your Outlook, you can recover it from the server:
* Select the `Deleted Items` folder
* On the `Home` menu, select `Recover Deleted Items From Server`
* Select the items that you want to recover, select `Restore Selected Items`, and then select `OK`

### Add Emoji in Email
Press Windows logo key + .(or ;) to open the Emoji Panel

## Markdown
### Comment
```markdown
(empty line)
[comment]: # (This actually is the most platform independent comment)
```

### Table
Tables aren't part of the core Markdown spec, but they are part of GFM and Markdown Here supports them.
```markdown
Colons can be used to align columns.

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

There must be at least 3 dashes separating each header cell.
The outer pipes (|) are optional, and you don't need to make the 
raw Markdown line up prettily. You can also use inline Markdown.

Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3
```

### Can I merge cells?
No. You can't merge cells in Markdown for now. But you can
* Embed raw html in the markdown file. Or
* Optionally you can copy the content to each cell instead of merge them.

### Inline HTML
You can also use raw HTML in your Markdown, and it'll mostly work pretty well.
```markdown
<dl>
  <dt>Definition list</dt>
  <dd>Is something people use sometimes.</dd>

  <dt>Markdown in HTML</dt>
  <dd>Does *not* work **very** well. Use HTML <em>tags</em>.</dd>
</dl>
```

### Escape
You can make a backtick (`) render within inline code by wrapping the code in double (or more) backticks (``)

```
`` `foo` ``
```

### Links
There are two ways to create links.
```markdown
* [I'm an inline-style link](https://www.google.com)
* [You can use numbers for reference-style link definitions][1]
* [I'm a reference-style link][Arbitrary case-insensitive reference text]
* Or leave it empty and use the [link text itself]

URLs and URLs in angle brackets will automatically get turned into links.
* http://www.example.com or 
* <http://www.example.com>.

Some text to show that the reference links can follow later.

[1]: http://slashdot.org
[arbitrary case-insensitive reference text]: https://www.mozilla.org
[link text itself]: http://www.reddit.com
```

### Images
```markdown
Here's our logo (hover to see the title text):

Inline-style: 
![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")

Reference-style: 
![alt text][logo]

[logo]: https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 2"
```

## VLC
### Lock Window Size
Select Tools ⇨ Preferences ⇨ Interface then uncheck the box Resize interface to video size. Finally, restart VLC.
When I started a video VLC was always maximized. The box was checked by default.

## Hugo

### Future Date
By default, Hugo will not render the contents with publishdate/date param set to a future date.

**Development**
* Use `hugo serve -F` or `hugo serve --buildFuture` to render them in develop mode.

**Production**

### Automatic Deploy Everyday
**Setup a build hook in Netlify**
* Login to [Netlify](https://app.netlify.com/) using Github account
* Select the site
* Site settings > Build & deploy > Build hooks > Add build hook > Name: Publish Blog Everyday
* Copy the URL (https://api.netlify.com/build_hooks/your-token-here-xxx)

**Github**
Settings > Secrets > New repository secrets > 
* Name: Netlify_Blog_Token
* Value: (paste the build hook token here)

**Repository**
* Create a `.github/workflows` directory in your repository
* Create a new file `netlify.yml` in the folder
* Create the workflow
```yaml
name: Netlify Scheduled Post

on:
  schedule:
  # At 00:15; see https://crontab.guru
    - cron: "15 0 * * *"

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Trigger Netlify Hook
        run: curl -s -X POST "https://api.netlify.com/build_hooks/${TOKEN}"
        env:
          TOKEN: ${{ secrets.NETLIFY_BLOG_TOKEN }}
```
* Commit and push it into Github
* You can verify in Github > Your repository > Actions > Workflows 

Note: 
* 8:00am UTC(+00:00) = 12:00am PST(-08:00)


Problem: 
When I push the .github/workflows/netlify.yml to Github, the following errors:
```
error: failed to push some refs to 'https://github.com/AngangGuo/blog.git'
!	refs/heads/master:refs/heads/master	[remote rejected] 
(refusing to allow an OAuth App to create or update workflow 
`.github/workflows/netlify.yml` without `workflow` scope)
```
see [refuse to update workflow](https://stackoverflow.com/questions/64059610/how-to-resolve-refusing-to-allow-an-oauth-app-to-create-or-update-workflow-on)

Solution:
Github > Settings > Developer settings > Generate new token > (make sure workflow is checked)

Goland:
Settings > Version Control > Github > 
Then push again from Goland.

## Linux
### How to change the Ubuntu Grub boot order?
I have a Windows / Ubuntu dual boot system. The default option is to boot into Ubuntu first.
I can change the default boot order to Windows by using [Grub Customizer](https://launchpad.net/grub-customizer). 

Starting with Ubuntu 19.10, Grub Customizer is available in the Universe repository. 
You can install it from Software Center. Or you can install it from command line:
```
sudo apt install grub-customizer
```

### Commands
```
mkdir -p ~/go/src/github.com/AngangGuo/rl && cd ~/go/src/github.com/AngangGuo/rl
```

## Other
### W3C Time Format
```
1997-07-16T19:20:30.45+01:00
2021-09-01T00:00:00+00:00
2021-09-01T59:59:59+00:00
2021-01-13T16:55:05-08:00
```

## GrapQL
[gqlgen](https://gqlgen.com/getting-started/) should be the preferred library in Go.
See [Feature Comparison](https://gqlgen.com/feature-comparison/)

## Zoom
### Useful commands
Alt + R: Record on this computer
Ctrl + Alt + Shift + H: Hide floating meeting controls

### How to change your profile picture?
See [doc](https://support.zoom.us/hc/en-us/articles/201363203-Customizing-your-profile#h_01F6MWFRY3D62ANBVYSB7ZNNS7)

## Useful Links
### Video
* Videojs and related video processing articles: https://cloud.tencent.com/developer/article/1615717

### Tools
* [Ventoy](https://www.ventoy.net/en/index.html) is an open source tool to create bootable USB drive for ISO/WIM/IMG/VHD(x)/EFI files. 