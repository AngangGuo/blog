---
title: "Tips - Git Version Control"
date: 2020-04-27T15:32:20-07:00
categories:
 - Tech
 - Tools
tags:
 - Git
 - Gitlab
 - Github
draft: false
---

## Git

### Adding Files
```
// To stage all files(including all files in subdirectories) in your repository, 
// which includes all new, modified, and deleted files
git add -A

// To stages files in the current directory and not any subdirectories
// It also removes a file from your repository if it no longer exists in the project
git add .

// By adding the --ignore-removal option, which will only stage new and modified files:
git add --ignore-removal .

// To stage only the Modified and Deleted Files, but not any new files
git add -u

// Adding file by wildcard
git add *.html

// To add all JavaScript files, including those in subdirectories:
git add **/*.js
```

### Tag
```
// lightweight tag
git tag v0.1

// create annotated tag
git tag -a v0.2 -m "my version 0.2"

// show tags
git tag

// show tag details
git show v1.0
```
### How to change remote URL from Gitlab to Github?
```
$ git remote -v
origin  https://gitlab.com/angang/renewalfamily.git (fetch)
origin  https://gitlab.com/angang/renewalfamily.git (push)

$ git remote set-url origin https://github.com/AngangGuo/RenewalFamily.git
$ git remote -v
origin  https://github.com/AngangGuo/RenewalFamily.git (fetch)
origin  https://github.com/AngangGuo/RenewalFamily.git (push)

git push -u origin master
```

### How to rename a git tag
To rename git tag `v0.2` to `v1.0`
```
// create a new tag
$ git tag v1.0 v0.2
$ git push --tags
Total 0 (delta 0), reused 0 (delta 0)
To https://github.com/AngangGuo/donation.git
 * [new tag]         v1.0 -> v1.0

// delete old tag
$ git tag -d v0.2
$ git push origin :refs/tags/0.2
remote: warning: Deleting a non-existent ref.
To https://github.com/AngangGuo/donation.git
 - [deleted]         0.2


```


### How to rewrite the most recent commit message?
**Commit has not been pushed online**
```
git commit --amend -m "New commit message"
// or open the editor by using the following command
git commit --amend
```

**Amending the message of the most recently pushed commit**

Follow the steps above to amend the commit message.
Use the `push --force` command to force push over the old commit.
```
$ git push --force example-branch
```

See [here](https://docs.github.com/en/github/committing-changes-to-your-project/changing-a-commit-message)

## Github
### Preview HTML File
If you want to see an HTML page on Github as a normal rendered HTML page, go to https://htmlpreview.github.io/

### Compare View
Every repository contains a Compare view, which allows you to compare the state of your repository across branches, tags, commits, time periods, and more. The compare view provides you with the same diff tooling that the Pull Request view does.

To get to the compare view, append `/compare` to your repository's path(eg. `https://github.com/AngangGuo/blog/compare`)

### How to pin items to your profile
You can pin gists, your own public repositories or others' repositories if you've made contributions to your profile.

1. In the top right corner of GitHub, click your profile photo, then click Your profile.
2. In the "Popular repositories" or "Pinned" section, click Customize your pins.
3. Select a combination of up to six repositories and/or gists to display.
4. Click Save pins.

## Gitlab
### How to sync `gitlab` and `github` repository?
See [gitlab doc][gitlab]

I moved my blog from `github` to `Gitlab` but want to keep my `github` repository. 
`gitlab` repository mirroring allows me to sync the two repositories.

Click `settings` of my blog project > Repository > Mirroring repositories section.

* Git repository URL: `https://Angang.Guo@github.com/AngangGuo/blog.git`
* Mirror direction: `Push`
* Password: xxx
* Click the `Mirror repository` button to save the configuration.

Note: Add your login name before the URL.



[gitlab]: https://docs.gitlab.com/ee/user/project/repository/repository_mirroring.html