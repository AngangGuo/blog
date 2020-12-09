---
title: "Migrating a Website to Netlify"
date: 2020-12-06T12:47:19-08:00
categories:
 - Tech
 - Website
tags:
 - Git
 - Github 
 - Netlify
draft: false
---

## Preparing Website Files In Github
Signup or login to [Github](https://github.com/)

### Create Repository In Github
1. Click `Create Repository`
   
   ![Create Repository](/images/2020/github-create-repository.jpg)

2. Repository name: `renewalfamily/website` > Click `Create Repository`
   
   ![Renewal Family website Repository](/images/2020/github-new-repository.jpg)

3. Github shows the instructions
   
   ![New Repository Instruction](/images/2020/github-repo-instruction.jpg)

### Upload Website Files To Github
All the website files are in the `C:\andrew\prj\go\src\renew-website>` folder. Go to the folder and execute the following [git](https://git-scm.com/download/win) commands from PowerShell to upload files to Github:

```
git init
git add -A
git commit -m "commit all files"
git remote add origin https://github.com/renewalfamily/website.git
git branch -M main
git push -u origin main
```

All the files are in Github and ready to be deployed from Netlify

## Deploy The Website From Netlify
### Signup A New Account
1. Signup Using Github
   
   ![Netlify Signup](/images/2020/netlify-signup.jpg)

2. Authorize From Github Account
   
   ![Netlify Signup Authorize](/images/2020/netlify-github-auth.jpg)

### Deploy The Website
1. Login to your account and click `New Site From Git`
   
   ![Netlify New Site](/images/2020/netlify-newsite.jpg)

2. Select `Github`
   
   ![Netlify New Site From Github](/images/2020/netlify-newsite-github.jpg)

3. Give Netlify permission to access your Github repository
   
   ![Netlify Request Github Permission](/images/2020/netlify-newsite-github-auth.jpg)

4. Install Netlify into `renewalfamily/website`
   
   ![Netlify Install](/images/2020/netlify-newsite-install.jpg)

5. Confirm Github password
   
   ![Netlify Install](/images/2020/netlify-newsite-install-1.jpg)

6. Select the repository and create a new site from it
   
   ![Netlify Install](/images/2020/netlify-newsite-pick-repo.jpg)

7. Deploy Settings
   Since there're only static files in the foler and no site generator used, ignore the `Basic Build Settings` section. Click `Depoly site`
   
   ![Netlify Install](/images/2020/netlify-deploy-site.jpg)
   
8. The website will be deployed after a minute. 
   The website is in Netlify subdomain: `[name-of-your-site].netlify.app`.
   See the optional customer domain setup section if you want to use your own domain name.

   ![Netlify Install](/images/2020/netlify-site-deployed.jpg)

9. Check the website https://hardcore-lumiere-1f990a.netlify.app/

   ![Netlify Install](/images/2020/netlify-deployed-website.jpg)

### Setup DNS from your DNS provider
To use a customer domain, you will need to add DNS records on your provider 
to point your domain or subdomain to your site on Netlify.

If your DNS provider does not support CNAME-style resolution for apex domains, 
you must configure your domain with a single-server `A` record:
1. Find your DNS provider's DNS record settings for your apex domain, such as `renewalfamily.org`.
2. Add an `A` record. Depending on your provider, leave the host field empty or enter `@`.
3. Point the record to Netlify's load balancer IP address: `104.198.14.52`.
4. Save your settings. 

If your DNS provider supports CNAME-style resolution for apex domains(recommended), 
find and follow their instructions to point the apex domain directly to your Netlify subdomain `hardcore-lumiere-1f990a.netlify.app`.

It may take a full day for the settings to propagate across the global Domain Name System.

### Setup Customer domain
1. Enter your domain name and click `Verify`
    
   ![Netlify Install](/images/2020/netlify-customer-domain.jpg)

2. Select `Yes, Add domain`

   ![Netlify Install](/images/2020/netlify-add-domain.jpg)

3. Check customer domain

   ![Netlify Install](/images/2020/netlify-check-dns.jpg)

4. Set up HTTPS

   ![Netlify Install](/images/2020/netlify-https-check.jpg)


### How to delete a deployed website from Netlify
Site Overview > Site Settings > General > Click `Delete this site` and type in the site name will delete the website.

## Troubleshooting
### Setting Git Branch

```
PS C:\andrew\prj\go\src\renew-website> git remote add origin https://github.com/renewalfamily/website.git
PS C:\andrew\prj\go\src\renew-website> git push -u origin
fatal: The current branch master has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin master

PS C:\andrew\prj\go\src\renew-website> git branch -M main
PS C:\andrew\prj\go\src\renew-website> git push -u origin main
```

### Website Doesn't Show Out
I have `index-cn.html`, `index-tw.html` and `index-cn.html` files in root folder but no `index.html`.
Netlify will show `Page Not Found` error if `index.html` is missing. 

   ![Page Not Found](/images/2020/netlify-404.jpg)

Add the following `index.html` to redirect home page to `index-cn.html`.

```
<!doctype html>
<html lang="en">
<head>
    <meta http-equiv="refresh" content="0; URL=index-cn.html" />
</head>
<body>
    Redircting to index-cn.html...
</body>
</html>
```

### Page Not Found
After moving to the new server, the link changed from `https://www.renewalfamily.org/v2` to `https://www.renewalfamily.org/`.
Some of the link may broken and Netlify will show the default "Page Not Found" web page.

To customize the page, create a `404.html` in your root folder:
```
<div class="col text-center">
   <h3>Sorry / 对不起</h3>
   <p>We couldn't find that page.<br>
   找不到网页。
   </p>
   <p><a href="https://www.renewalfamily.org/index-en.html">Go to "Renewal Family" home page</a><br>
       <a href="https://www.renewalfamily.org/index-cn.html">进入《更新家庭网站》主页</a>
   </p>

</div>
```

### Screenshot of git command

According to the instruction, upload the files by using [git](https://git-scm.com/download/win) from PowerShell command line:
```
PS C:\andrew\prj\go\src\renew-website> git init
Initialized empty Git repository in C:/andrew/prj/go/src/renew-website/.git/

PS C:\andrew\prj\go\src\renew-website> git add -A

PS C:\andrew\prj\go\src\renew-website> git commit -m "init commit"
[master (root-commit) 9d68b77] init commit
 153 files changed, 10834 insertions(+)
 create mode 100644 awaken-cn.html
 create mode 100644 awaken-en.html
 create mode 100644 awaken-tw.html
 ... ...
 
PS C:\andrew\prj\go\src\renew-website> git remote add origin https://github.com/renewalfamily/website.git

PS C:\andrew\prj\go\src\renew-website> git push -u origin main
Logon failed, use ctrl+c to cancel basic credential prompt.
Username for 'https://github.com': renewalfamily
Password for 'https://renewalfamily@github.com':
Enumerating objects: 158, done.
Counting objects: 100% (158/158), done.
Delta compression using up to 4 threads
Compressing objects: 100% (158/158), done.
Writing objects: 100% (158/158), 450.97 MiB | 4.93 MiB/s, done.
Total 158 (delta 33), reused 0 (delta 0)
remote: Resolving deltas: 100% (33/33), done.
remote: warning: GH001: Large files detected. You may want to try Git Large File Storage - https://git-lfs.github.com.
remote: warning: See http://git.io/iEPt8g for more information.
remote: warning: File static/moshi/Lesson9-TheCross.mp3 is 70.78 MB; this is larger than GitHub's recommended maximum file size of 50.00 MB
remote: warning: File static/moshi/Lesson9.m4a is 52.46 MB; this is larger than GitHub's recommended maximum file size of 50.00 MB
To https://github.com/renewalfamily/website.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
PS C:\andrew\prj\go\src\renew-website>

```
