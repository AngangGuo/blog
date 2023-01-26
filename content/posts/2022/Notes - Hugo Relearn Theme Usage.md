---
title: "Notes   Hugo Relearn Theme Usage"
date: 2022-10-03T15:08:48-07:00
categories:
  - Tech
  - Church
tags:
  - Hugo
  - Instruction
draft: false
---

Learn(and Relearn) theme works with a page tree structure to organize content: 
All contents are pages, which belong to other pages.

## Create a new site
```
hugo new site churchMedia
cd churchMedia
git init
git submodule add https://github.com/McShelby/hugo-theme-relearn.git themes/relearn
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
