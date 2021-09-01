---
title: "Creating A Blog Using Hugo"
date: 2020-02-28T22:31:20-08:00
categories:
 - Tech
 - Web
tags:
 - Hugo
 - Blog
 - Go
draft: false
---

[Hugo](https://gohugo.io/) is one of the most popular open-source static site generators. 
I just heard that the new Bootstrap 5 will use Hugo to generate their document. 
With its amazing speed and flexibility, Hugo makes building websites fun again.
You can build a beautiful website in less than an hour.

Hugo is written in [Go programming language](https://golang.org/) which I like the most.
You can write the blog content by using [Markdown](https://daringfireball.net/projects/markdown/) syntax, 
which means you can edit the blog in any text editor.

You can use any text editor for the markdown, even the simplest Notepad. 
But good code editors share many of the same great major features you might expect, 
including auto-completion (code completion), git integration, and markdown plugin support.
I recommend you can use the most popular free editor [Microsoft Visual Studio Code](https://code.visualstudio.com/) or
the commercial editor [JetBrains Goland](https://www.jetbrains.com/go/) which is the best for my Go(including Hugo) development.


## Download Hugo
Downloading the Hugo binary file from [Hugo Release Page](https://github.com/gohugoio/hugo/releases).
See [Install Hugo](https://gohugo.io/getting-started/installing/) for details.

* Add Hugo Executable to your path
* Verify the installation
```
C:\andrew>hugo version
Hugo Static Site Generator v0.65.3/extended windows/amd64 BuildDate: unknown
```
Note: You can download the **"extended"** version if you need Sass/SCSS support.

## Create Blog Website
```
C:\andrew> mkdir prj
C:\andrew> cd prj

C:\andrew\prj> hugo new site blog
Congratulations! Your new Hugo site is created in C:\andrew\prj\blog.

Just a few more steps and you're ready to go:

1. Download a theme into the same-named folder.
   Choose a theme from https://themes.gohugo.io/, or
   create your own with the "hugo new theme <THEMENAME>" command.
2. Perhaps you want to add some content. You can add single files
   with "hugo new <SECTIONNAME>\<FILENAME>.<FORMAT>".
3. Start the built-in live server via "hugo server".

Visit https://gohugo.io/ for quickstart guide and full documentation.

```

## Install Theme
Hugo website doesn't come with any default theme as for now. See the [discuss](https://github.com/gohugoio/hugo/issues/2864).

You need to install a theme for it to work. You can create your own theme, or you can choose a theme from [Hugo themes page](https://themes.gohugo.io/).

I like the [MemE theme](https://github.com/reuixiy/hugo-theme-meme/) and will install it for my blog website. 
If you do not have git installed, you can download the theme and extract that .zip file to `themes` folder.

Make sure [git](https://git-scm.com/download) is installed on your system. I'll use git to add the theme in this example. 

```
C:\andrew\prj\blog>git version
git version 2.25.1.windows.1

C:\andrew\prj\blog>git init
Reinitialized existing Git repository in C:/andrew/prj/blog/.git/

C:\andrew\prj\blog>git submodule add https://github.com/reuixiy/hugo-theme-meme.git themes/meme
Cloning into 'C:/andrew/prj/blog/themes/meme'...
remote: Enumerating objects: 479, done.
remote: Counting objects: 100% (479/479), done.
remote: Compressing objects: 100% (283/283), done.
remote: Total 3241 (delta 197), reused 381 (delta 128), pack-reused 2762R                           9/3241), 6.18 MiB | 5.67 MiB/s
Receiving objects: 100% (3241/3241), 8.02 MiB | 5.95 MiB/s, done.
Resolving deltas: 100% (1702/1702), done.
warning: LF will be replaced by CRLF in .gitmodules.
The file will have its original line endings in your working directory
```

Note: Use this command to update the theme: `git submodule update --rebase --remote`

## Config

Themes often come with a directory called `exampleSite`, which contains example content and an example settings file. 

To activate the theme, copy the provided `config.toml` file into your blog root.
### Replace the `config.toml` file.
```
C:\andrew\prj\blog>copy themes\meme\config-examples\en-us\config.toml
Overwrite C:\andrew\prj\blog\config.toml? (Yes/No/All): Yes
        1 file(s) copied.
```

### Modify the theme configuration
* Author's information section
* Website title
* (Option) And many other parameters to fine tune the web pages

## Add a blog page
Use `hugo new "blogname.md"` to create a Markdown blog.

```
C:\andrew\prj\blog>hugo new "posts/2020/Creating A Blog Using Hugo.md"
C:\andrew\prj\blog\content\posts\2020\Creating A Blog Using Hugo.md created
```

### Modify the page front matters
You can use YAML, TOML, or JSON format in front matters. Here is a YAML example:
```
---
title: "Creating A Blog Using Hugo"
date: 2020-02-28T22:31:20-08:00
categories:
 - Tech
 - Web
tags:
 - Hugo
 - Blog
 - Theme
 - Go
draft: true
---
```

Note: YAML and TOML file support single line comments. You can comment a line out by adding `#` in front of the line

### Add Contents to the pages
Write the article by using Markdown syntax.

### Run the develop server
Go to `http://localhost:1313`, the page will refresh whenever you content changes.

```
C:\andrew\prj\blog>hugo server -D
Building sites …
                   | EN-US
-------------------+--------
  Pages            |    10
  Paginator pages  |     0
  Non-page files   |     0
  Static files     |    10
  Processed images |     0
  Aliases          |     1
  Sitemaps         |     1
  Cleaned          |     0

Built in 219 ms
Watching for changes in C:\andrew\prj\blog\{archetypes,content,data,layouts,static,themes}
Watching for config changes in C:\andrew\prj\blog\config.toml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

## Build the site
When you finished editing your pages, update the draft status from `draft: true` to `draft: false`.

Build the whole blog site by using `hugo` command. The compiled site will be in `public` folder.

```
C:\andrew\prj\blog>hugo
Building sites …
                   | EN-US
-------------------+--------
  Pages            |     7
  Paginator pages  |     0
  Non-page files   |     0
  Static files     |    10
  Processed images |     0
  Aliases          |     1
  Sitemaps         |     1
  Cleaned          |     0

Total in 695 ms
```

## Go live
Upload the files in `public` folder to your website. Your new blog website will go live.

The easiest is to use [Vercel(ZEIT) Now](https://vercel.com/docs), see [example](https://github.com/zeit/now/tree/master/examples/hugo)

See [this post](/posts/2020/host-static-website-on-gitlab-pages/) on how to host your blog on Gitlab.

## FAQ
### How can I link within a page?
Here is the example on how to use anchor to link within a page.

```markdown
### Add anchor at the end of the text you want to link to {#my-anchor}

Add a link to the anchor by using the same markdown link syntax [Link to the anchor within the page]({{</* ref "#my-anchor" */>}})
```

### How can I add YouTube video to my page?
By using Hugo `Shortcode`, Copy the YouTube video ID that follows v= in the video’s URL and pass it to the youtube shortcode:
```
{{</* youtube qtIqKaDlqXo */>}}
```
{{< youtube qtIqKaDlqXo >}}

### How to add image in a page?
Copy the images into `blog/static/images` folder or its sub-folder. Use the following format to add the image in the blog:
```markdown
![CSS load error](/images/2020/css-load-error.PNG)
```
The Absolute URL or Relative URL settings may cause image loading problem. 
See [here]({{< ref "#css-load-error" >}}) for more details.

### How to add a link with space in it?
It's better to use dash or underscore to separate the words in file name or link instead of spaces.

If you do have space in the link or file name, you can use url encode the link like this:
```
![T51A Walkie Talkie](/images/2020/T51A%20Walkie%20Talkie.PNG)
```

### How to pin a post at the top of the home page?
Use `weight` in front matter for ordering your content in lists.
Lower weight gets higher precedence.
So content with lower weight will come first.
If set, weights should be non-zero, as 0 is interpreted as an unset weight.

## Troubleshooting
### Why my new page doesn't show out?
I create a new post page using the following command but the page doesn't show out.
```
hugo new "Quick Video Editing Using FFmpeg"
```

**Solution**
The `markdown` page created by `Hugo` must end with `.md`. By adding `.md` the page can be showed out: 
```
hugo new "Quick Video Editing Using FFmpeg.md"
```

### `page not found` in development machine?
If the `baseURL` is a subdomain, such as `baseURL="https://angang.gitlab.io/blog/"`, 
when running development server using `hugo server`, `localhost:1313` will show `404 page not found` error.

**Solution**
1. Set the development domain to `localhost` from command line(Prefer):
```
// localhost:1313
hugo server --baseURL=localhost
```

2. Set a temporary domain for development
Caution: Change the settings back to subdomain when you push the code to `gitlab`.
```
# Site Settings
# URL for DEV
baseURL = "https://angang.gitlab.io/"
# URL for production
#baseURL = "https://angang.gitlab.io/blog/"
```

The better solution can be using relative URLs as shown [below]({{< ref "#css-load-error" >}})

### Why the generated static site can't load CSS/JS files ? {#css-load-error}
Using `hugo` to generate the static files into `public` folder. 
It shows the following errors when serving it from `localhost` by using Goland browser.

```text
Refused to apply style from 'http://localhost/css/meme.min...css' 
because its MIMI type ('text/html') is not a supported stylesheet MIMI type, 
and strict MIMI checking is enabled.
...
```

![CSS load error](/images/2020/css-load-error.PNG)

If you want to have your site to be installed in different places or using different domain names,
you need to use relative URL instead of the specific URL(https://blog.angang.ca/).

Change all these settings together when using relative URL.

`config.toml` for Absolute URL
```toml
# Site Settings
baseURL = "https://blog.angang.ca/"
relativeURLs = false

# Image Hosting
enableImageHost = true
imageHostURL = "https://blog.angang.ca/"
```

`config.toml` for Relative URL
```toml
# Site Settings
baseURL = ""
relativeURLs = true

# Image Hosting
enableImageHost = false
imageHostURL = "/"
```

For more details see also [here](https://discourse.gohugo.io/t/gohugo-without-baseurl-or-with-relative-url-only/2779)
and [here](https://discourse.gohugo.io/t/mime-type-text-plain-is-not-a-supported-stylesheet-mime-type-and-strict-mime-checking-is-enabled/16435/11)


