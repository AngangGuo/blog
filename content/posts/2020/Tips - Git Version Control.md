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

### How to remove the last commit from Github?
```
// Reset local repository to previous commit
// If you don't want to keep the file changes, use --hard instead of --soft
// If you want to keep the changes to current files
git reset --soft HEAD^ // use HEAD~2 for last 2 commit

// Push the changes to Github by force
git push origin --force
```
### How to upload large files into Github?
Note: GitHub blocks pushes that exceed 100 MB; Warning for files larger than 50 MB

* Download and install Git Large File Storage from https://git-lfs.github.com/
* Verify the installation from Git Bash: `git lfs install`
* Change your current working directory to an existing repository
* Associate the file type in your repository with Git LFS: `git lfs track "*.m4a"`
* Add the `.gitattributes` file along with other files which need to be committed and push the changes.  
* push to Github: 
```
git add .gitattributes
git add audio.m4a
git commit -m "add my audio file"
git push origin main
```

Note: 
* Netlify support Github large files. See https://docs.netlify.com/large-media/overview/
* Maybe you can use Audacity or other software to convert the audio files to lower 

### How to clear out the history of a Git/Github repository
See: [repo-reset](https://gist.github.com/heiswayi/350e2afda8cece810c0f6116dadbe651) and 
[git-clearHistory](https://gist.github.com/stephenhardy/5470814)
```
-- First: Remove your history 
rm -rf .git

-- Second: Add your current content
git init
git add .
git commit -m "Initial commit"

-- Better delete the repository from Github and re-create it
-- This will delete any sensitive information and pull request, etc
git remote add origin git@github.com:<YOUR ACCOUNT>/<YOUR REPOS>.git
git push origin master

-- Or if you want to keep the repository
-- Third: push to the github remote repos ensuring you overwrite history
git remote add origin git@github.com:<YOUR ACCOUNT>/<YOUR REPOS>.git
git push -u --force origin master // or git push --mirror --force
```

### How to create a task list?
Beginning with a dash, use spaces separate the square brackets.
```markdown
- [x] Watch TV
- [ ] Do homework
```
- [x] Watch TV
- [ ] Do homework

### How to reference a commit or issue in comment?
Every commit, issue or pull request has a uniqe URL or SHA hash code(commit only). 
Insert the URL or SHA into the comment directly, 
Github will automatically convert it into shortened links to the commit.

```
// for commit
Commit URL: https://github.com/jlord/sheetsee.js/commit/a5c3785ed8d6a35868bc169f07e40e889087fd2e =>	a5c3785
SHA: a5c3785ed8d6a35868bc169f07e40e889087fd2e => a5c3785

// for issue and pull request
Issue or pull request URL: https://github.com/AngangGuo/playiq/issues/4 => #4
# and issue or pull request number: #4 => #4
```

See [Autolinked references and URLs](https://docs.github.com/en/github/writing-on-github/autolinked-references-and-urls)

### How to add images to Github comments?
* Using Snipping Tool in Windows to get the image, copy to clipboard.
* Click on the right position in Github issue comments, paste the image in the comment directly

Or using drag and drop also works.

### How to mark Github issue as `bug`?
Open the issue, at the right hand of the page, you can assign `bug` label to mark the issue.

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