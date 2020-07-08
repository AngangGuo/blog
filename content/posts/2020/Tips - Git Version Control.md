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