---
title: "Notes   Miscellaneous"
date: 2021-07-30T08:49:20-07:00
categories:
  - Tech
  - Other
tags:
  - Outlook
  - Markdown
draft: false
---

## Teams
### How to quote message in Teams desktop?
Quote using a keyboard shortcut
* Copy the message
* On your keyboard press “Shift” + “>”
* Paste the original message and then type your own message
* Press Enter twice and to type your message after the quote

## Outlook
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

## Command Line
### Execute multiple commands on one line
* Windows 98: `go build | try`
* &: separate multiple commands on one command line.
* &&: run the command following && only if the command preceding the symbol is successful. `mkdir -p newApp && cd newApp`
* ||: run the command following || only if the command preceding || fails.
* For Powershell or Bash(semicolon): `go build; .\try.exe`

### Change character encoding
Beginning in PowerShell 5.1, the redirection operators (> and >>) call the Out-File cmdlet, 
which is UTF-16 encoding by default. To change the encoding to UTF-8:

`$PSDefaultParameterValues['Out-File:Encoding'] = 'utf8'`

`.\plan-b.exe | Out-File plan.txt -encoding utf8`

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
### How to change your profile picture?
See [doc](https://support.zoom.us/hc/en-us/articles/201363203-Customizing-your-profile#h_01F6MWFRY3D62ANBVYSB7ZNNS7)

## Useful Links
* Videojs and related video processing articles: https://cloud.tencent.com/developer/article/1615717