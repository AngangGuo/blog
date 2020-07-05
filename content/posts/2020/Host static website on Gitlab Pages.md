---
title: "Host Static Website on Gitlab Pages"
date: 2020-03-08T09:00:59-07:00
categories:
 - Tech
 - Web
tags:
 - Gitlab
 - Blog
draft: false
---
There're lots of free services that you can host your static website: 
* [GitHub Pages](https://pages.github.com/)
* [Netlify](https://www.netlify.com/)
* [Gitlab](https://docs.gitlab.com/ee/user/project/pages/)

And many more. See [Hugo Hosting & Deployment](https://gohugo.io/hosting-and-deployment/)

I'll show you how to deploy your website to Gitlab Pages. 
GitLab Pages is a feature that allows you to publish static websites directly from a repository in GitLab.
It's the easiest way to get your website up and running.

See [Creating a blog using Hugo](../creating-a-blog-using-hugo/) first if you don't know how to create a website.

Continue from the last post. 
 
The key to having our website up and running as expected is the GitLab CI configuration file, 
called `.gitlab-ci.yml`.

## Create Config File 
Create the following `.gitlab-ci.yml` file in the blog folder.

```
# this image is from gitlab that is Hugo normal version
# image: registry.gitlab.com/pages/hugo:latest

# This is the Hugo Extended version, can be used to build SCSS/SASS
image: monachus/hugo

variables:
  GIT_SUBMODULE_STRATEGY: recursive

pages:
  script:
  - hugo
  artifacts:
    paths:
    - public
  only:
  - master
```

## Create Project On Gitlab
* Login to [Gitlab](https://gitlab.com/users/sign_in)
* Click `New Project` and fill in the project name `blog`
* Click `Create Project`

Git global setup
```
git config --global user.name "your name here"
git config --global user.email "your email here"
```

Push the Hugo blog into Gitlab repository
```
cd blog
git remote rename origin github-origin
git remote add origin https://gitlab.com/angang/blog.git
git add .
git commit -m "add .gitlab-ci.yml"
git push -u origin --all
git push -u origin --tags
```

Notes: Goland prompted me for the Gitlab credential and I typed the wrong password.
* To Remove or update the Credentials stored in Windows 10, go to 
`Control Panel\All Control Panel Items\Credential Manager\Windows Credentials`,
 Remove the git:https://gitlab.com item.
* To remove the credentials from Goland, go to `Settings>>Appearance & Behavior>>System Settings>>Passwords>>Reset`.

## Wait for Your Page to Build 
You can now follow the CI agent building your page at 
`https://gitlab.com/<YourUsername>/<your-hugo-site>/pipelines`.
   
After the build has passed, your new website is available at https://<YourUsername>.gitlab.io/<your-hugo-site>/.

## Resources
* [Gitlab Hugo CI Config Example][1]
* [Hugo Gitlab instruction][2]
* [Using Gitlab Hugo Template][3]


[1]: https://gitlab.com/pages/hugo/-/blob/master/.gitlab-ci.yml
[2]: https://gohugo.io/hosting-and-deployment/hosting-on-gitlab/
[3]: https://about.gitlab.com/blog/2019/02/20/start-using-pages-quickly/
[4]: https://angang.gitlab.io/blog/

## Troubleshooting
### Gitlab CI / CD Pipelines failed
When I config the `.gitlab-ci.yml` using `image: registry.gitlab.com/pages/hugo:latest` 
from the [Gitlab Hugo CI Config Example][1], the build fails.

Here is the error message:
```
 Running with gitlab-runner 12.8.0 (1b659122)
    on docker-auto-scale 72989761
 Using Docker executor with image registry.gitlab.com/pages/hugo:latest ...
 00:10
  Authenticating with credentials from job payload (GitLab Registry)
  Pulling docker image registry.gitlab.com/pages/hugo:latest ...
  Using docker image sha256:914e4898d5c647b86f93c54cb87fe82537774b4f0bfc58cc9c99a9bbe557cae1 for registry.gitlab.com/pages/hugo:latest ...
 Running on runner-72989761-project-17365930-concurrent-0 via runner-72989761-srm-1583893835-0315de42...
 00:02
 $ eval "$CI_PRE_CLONE_SCRIPT"
 00:03
  Fetching changes with git depth set to 50...
  Initialized empty Git repository in /builds/angang/blog/.git/
  Created fresh repository.
  From https://gitlab.com/angang/blog
   * [new ref]         refs/pipelines/125139993 -> refs/pipelines/125139993
   * [new branch]      master                   -> origin/master
  Checking out 63f5f3ad as master...
  Updating/initializing submodules recursively...
  Submodule 'themes/meme' (https://github.com/reuixiy/hugo-theme-meme.git) registered for path 'themes/meme'
  Cloning into '/builds/angang/blog/themes/meme'...
  Submodule path 'themes/meme': checked out '7020dcc4d74bafed78f34ba315fc1c13dd70db8e'
  Entering 'themes/meme'
 Authenticating with credentials from job payload (GitLab Registry)
 00:01
  $ hugo
  Building sites … ERROR 2020/03/11 02:31:44 Transformation failed: TOCSS: failed to transform "styles/main-rendered.scss" (text/x-scss): resource "scss/blog/scss/main.scss_b04c21e44835e7dcdc8cb03d103b5ef1" not found in file cache. 
  Check your Hugo installation; you need the extended version to build SCSS/SASS.
  Total in 214 ms
  Error: Error building site: logged 1 error(s)
  ERROR: Job failed: exit code 255
```

The message `Check your Hugo installation; you need the extended version to build SCSS/SASS.`
means we need Hugo Extended version to build this site(Required by Meme theme). 

**Solution:**

Use the image `image: monachus/hugo` from the [Hugo Gitlab instruction][2]

### The Page Looks strange
It seems the pages can't load the CSS file: 

`<link rel="stylesheet" href="/blog/css/meme.min.css" />`

After reading the [Using Gitlab Hugo Template][3], I noticed that the `baseURL` must be set as
`https://namespace.gitlab.io/project` in Gitlab.

**Solution:** 

Config the `baseURL` in `config.toml` as `baseURL = "https://angang.gitlab.io/blog/"`

### Why the website requires Gitlab login account?
When click the link to [my blog][4], Gitlab asks me to the account to login. 
You can't visit the blog without Gitlab account.

**Solution:**

In Settings >> General >> Pages access control >> 
Access control for the project’s static website, it must be “every one with access”

### Why the image doesn't show out?
When setting the `baseURL` in `config.toml` with subdomain, 
e.g. `baseURL = "https://angang.gitlab.io/blog/"`,
the image in `/static/images/2020/m32.jpg` doesn't show out.

The generated image source link is:
```
<img src="/images/2020/m32.jpg" alt="Midas M32" class="medium-zoom-image">
```

The error message shows 
```
GET https://angang.gitlab.io/image/2020/m32.jpg 404 (Not Found)
```

**Solution**

Set the following two parameters in `config.toml`:
```
enableImageHost = true
imageHostURL = "https://angang.gitlab.io/blog/"
```

The generated image source link will be changed as following:
```
<img src="https://angang.gitlab.io/blog/images/2020/m32.jpg" alt="Midas M32" class="medium-zoom-image">
```