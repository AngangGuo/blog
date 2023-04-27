---
title: "Notes   Hugo Relearn Theme Usage"
date: 2022-10-03T15:08:48-07:00
categories:
  - Tech
tags:
  - Hugo
  - Instruction
draft: false
---

Learn(and Relearn) theme works with a page tree structure to organize content: 
All contents are pages, which belong to other pages.

## Create a new site
### Create empty Hugo site
```
hugo new site rldoc
cd rldoc
```

### Add `Relearn` theme
Using git submodule if you don't have Go installed: 
```
git init
git submodule add https://github.com/McShelby/hugo-theme-relearn.git themes/relearn
```

Using the following commands if you want to use Go module instead of git submodule:
```
// YOUR GitHub URI
hugo mod init github.com/AngangGuo/rldoc
hugo mod get github.com/McShelby/hugo-theme-relearn
```

### Set theme for the website
Update config.toml
```
[module]
[[module.imports]]
    path = 'github.com/McShelby/hugo-theme-relearn'

# Change the default theme to be use when building the site with Hugo
theme = "hugo-theme-relearn"

# For search functionality
[outputs]
home = [ "HTML", "RSS", "SEARCH"]
```

### (Optional)
Update Go module
```
hugo mod get -u

// to add module requirements and sums:
go mod tidy
```

### Try it
Create some pages according to below instructions and run it

### (Optional)Upload to `Github.com`
Create a new repository in Github(rldoc)
```
git remote add origin https://github.com/AngangGuo/rldoc.git
git branch -M main
git push -u origin main
Username for 'https://github.com': AngangGuo
Password for 'https://AngangGuo@github.com': (use token instead of password)ghp_XdN...ldu3
```

## File Structure
### Folders
* The root folder is `project/content` folder which contains all your pages.
* `_index.md` is required in each folder, it’s your “folder home page”
* 

### Pages
There are three kinds of predefined pages:
(See [here](https://mcshelby.github.io/hugo-theme-relearn/cont/archetypes/) for more details)

1. Home Page<br>
A `Home` page is the starting page of your project. It’s best to have only one page of this kind in your project.

To create a home page, run this command: 
```
hugo new --kind home _index.md
```

2. Chapter<br>
A `Chapter` displays a page meant to be used as introduction for a set of child pages.

To create a chapter page, run the following command
```
hugo new --kind chapter <name>/_index.md
```

3. Default<br>
A `Default` page is any other content page.

To create a default page, run either one of the following commands
```
hugo new <chapter>/<name>/_index.md
hugo new <chapter>/<name>.md
```

## Images
### Location
All the images should be in the same folder as the document.

For example, there're two files in `exampleSite/content/basic/requirements` folder:
```
_index.md
magic.gif
```
Use this code to show the image from within `_index.md`: 
`![Magic](magic.gif?classes=shadow)`

### FontAwesome Icons
You can use FontAwesome icons directly in your markdown file.

Note:
* Only [FontAwesome Version 5](https://fontawesome.com/v5/search) works with Relearn theme
* Find the icon and copy the HTML code, e.g `<i class="fas fa-wifi"></i>` and paste it into your markdown file directly

## Multilingual & i18n
### Basic Configuration
```
# English is the default language
defaultContentLanguage = "en"

[Languages]
[Languages.en]
title = "Documentation for Media Team"
weight = 1
languageName = "English"

[Languages.zn]
title = "媒体组设备使用指南"
weight = 2
languageName = "简体中文"
```

## Sample Websites
* https://ksucs-hugo.russfeld.me/
* https://textbooks.cs.ksu.edu/cis526/
* [From git submodule to Hugo Modules using Netlify](https://chrisshort.net/from-git-submodule-to-hugo-modules-using-netlify/)
