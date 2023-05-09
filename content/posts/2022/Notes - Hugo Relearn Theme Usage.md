---
title: "Notes - Hugo Relearn Theme Usage"
date: 2022-10-03T15:08:48-07:00
categories:
  - Tech
tags:
  - Hugo
  - Instruction
draft: false
---

Hugo [Relearn](https://github.com/McShelby/hugo-theme-relearn) theme works with a page tree structure to organize content: 
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

### Change theme color
Add the following code into `config.toml`
```
themeVariant = [ "auto", "relearn-bright", "relearn-light", "relearn-dark", "Red", "Blue"]
themeVariantAuto = [ "relearn-light", "relearn-dark" ]
```

### Add menu shortcuts
```
[[menu.shortcuts]]
name = "<i class='fab fa-fw fa-github'></i> GitHub repo"
identifier = "ds"
url = "https://github.com/McShelby/hugo-theme-relearn"
weight = 10

[[menu.shortcuts]]
name = "<i class='fas fa-fw fa-tags'></i> Tags"
url = "tags/"
weight = 40
```

### Change site logo
Create `logo.html` in /layouts/partials

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
Create a new repository in GitHub(rldoc)
```
git remote add origin https://github.com/AngangGuo/rldoc.git
git branch -M main
git push -u origin main
Username for 'https://github.com': AngangGuo
Password for 'https://AngangGuo@github.com': (use token instead of password)ghp_XdN...ldu3
```
## Settings
### Showing ">" before the parent menu items
```
# If set to true, the menu in the sidebar will be displayed in a collapsible tree view.
collapsibleMenu = true
```

### Hidden Pages
Hidden pages and all its children are hidden in the menu, arrow navigation and children shortcode.

You can visit the hidden page by using the direct link, or show the hidden children pages when `showhidden="true"`.

Hidden page setings
```
disableSearchHiddenPages = true
disableSeoHiddenPages = true
disableTagHiddenPages = true
```

```
# Hidden settings in Frontmatter
+++
title = "My Secret Page"
hidden = true
+++
```

```
# Show hidden children on this pages
{{%/* children showhidden="true" */%}}
```

## File Structure
### Folders
* The root folder is `project/content` folder which contains all your pages.
* `_index.md` is required in each folder, it’s your “folder home page”

### What's the page bundles?
Do you know the difference between using `index.md` and `_index.md`?
See [here](https://gohugo.io/content-management/page-bundles/)

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

### List the child pages(sub-folders) in the parent page
You can use the `children` shortcode to list the child pages 
```
// list only the page title
{{%/* children */%}}

// list page title with description
{{%/* children description="true" */%}}
```

See [here](https://mcshelby.github.io/hugo-theme-relearn/shortcodes/children/index.html)

### Link to other pages
* Link page in the same folder
```
[AWARDCO](awardco/) - Service Anniversary Platform.
```
* Link page in other folder
```
[Gift Card](../management/giftcard/) - Reward associates with gift card. 
```

## Attachments
The shortcode lists files found in a specific folder. 
The name of the folder depends on your page type (either branch bundle, leaf bundle or page).

### Folder name
* For simple pages, attachments must be placed in a folder named `your-page-name.files`.
* If your page is a branch or leaf bundle, attachments must be placed in a nested `_index.files` or `index.files` folder, accordingly.

### Show attachment files
```
// simplest format
{{%/* attachments /*/%}}

// customized title and selected file format
{{%/* attachments title="Related **files**" pattern=".*\.(pdf|mp4)" /*/%}}
```

See [here](https://mcshelby.github.io/hugo-theme-relearn/shortcodes/attachments/index.html)

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

### Font Awesome Icons
The `icon` shortcode displays icons using the Font Awesome library.

You can use Font Awesome icons directly in your markdown file.
* Browse through the available icons in the [Font Awesome Gallery](https://fontawesome.com/v5/search?m=free). 
* Once find the specific icon, copy the icon name and paste into the Markdown content, e.g. `{{%/* icon heart */%}}`
* Alternatively you can copy the HTML code, e.g `<i class="fas fa-heart"></i>` and paste it into your markdown file.

Note: Only Font Awesome version 5 works with the Relearn theme

### How to change site logo?
To change the site logo, create logo.html in /layouts/partials, edit the file contents and save it.
The content of this file will show as site logo of the page.

### Change the favicon
If your favicon is an SVG, PNG or ICO, just drop off your image in your local `static/images/` folder and name it `favicon.svg`, `favicon.png` or `favicon.ico` respectively. (Note: static/favicon.ico also works)

If no favicon file is found, the theme will look up the alternative filename `logo` in the same location and will repeat the search for the list of supported file types.

If you need to change this default behavior, create a new file in `layouts/partials/` named `favicon.html`. Then write something like this:
```
<link rel="icon" href="/images/favicon.bmp" type="image/bmp">
```

See [here](https://mcshelby.github.io/hugo-theme-relearn/basics/customization/index.html)

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

## Common Commands
### Update Go module
```
hugo mod get -u

// to add module requirements and sums:
go mod tidy
```

### Upload project to `Github.com`
Create a new repository in Github(rldoc)
```
git remote add origin https://github.com/AngangGuo/rldoc.git
git branch -M main
git push -u origin main
Username for 'https://github.com': AngangGuo
Password for 'https://AngangGuo@github.com': (use token instead of password)ghp_XdN...ldu3
```

## Sample Websites
* https://ksucs-hugo.russfeld.me/
* https://textbooks.cs.ksu.edu/cis526/
* [From git submodule to Hugo Modules using Netlify](https://chrisshort.net/from-git-submodule-to-hugo-modules-using-netlify/)
