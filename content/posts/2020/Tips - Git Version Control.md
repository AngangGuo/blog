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

### Git Branch
```
git branch // list all branches; = git branch --list
git branch new-branch // create a new branch
git checkout -b new-branch main
git checkout new-branch // switch to new-branch
git branch -d old-branch // delete old-branch; there'll be error if the branch hasn't been merged
git branch -D old-branch // force delete the old-branch

git push origin --delete old-branch // delete the old-branch in remote repos
```

### Git Merge
* fast-forward merge
```
git checkout -b new-feature main
// add / modify / delete files in this new branch and commit them
git checkout main
git merge new-feature
git branch -d new-feature
```

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

### How to make git forget a tracked file?
```
git update-index --skip-worktree <file> // ignore the file
git update-index --no-skip-worktree <file> // to cancel
```
See [here](https://stackoverflow.com/questions/1274057/how-do-i-make-git-forget-about-a-file-that-was-tracked-but-is-now-in-gitignore)

### What's the difference between `assume-unchanged` and `skip-worktree`?
* `--assume-unchanged` assumes that a developer shouldn’t change a file. This flag is meant for improving performance for not-changing folders like SDKs.
* `--skip-worktree` is useful when you instruct git not to touch a specific file ever because developers should change it. For example, if the main repository upstream hosts some production-ready configuration files and you don’t want to accidentally commit changes to those files, --skip-worktree is exactly what you want.

See [here](https://stackoverflow.com/questions/13630849/git-difference-between-assume-unchanged-and-skip-worktree/13631525#13631525)

### How to get the remote URL of a repository?
```
// one of the following commands
git remote -v
git config --get remote.origin.url
git ls-remote --get-url
git remote show origin
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
### How to add your project into Github?
```
git remote add origin https://github.com/YourAccountName/your-project.git
git branch -M main
git push -u origin main
```

### How to import repository from GitLab?
Go to your repositories > New > Import a repository 
* Your old repository's clone URL: https://gitlab.com/angang/shareverses
* Your new repository name: shareverses
* Begin Import
* Fill in credentials info and start import

See [here](https://documentation.its.umich.edu/node/4001) and [here](https://docs.github.com/en/migrations/importing-source-code/using-github-importer/importing-a-repository-with-github-importer)

### How to use GitHub personal access token?
* Settings > Developer settings > Personal access tokens > Generate new token > (write down it)
* When `git push` your commits, using personal access tokens instead of your password
```
PS C:\> git push origin --delete db-branch
Username for 'https://github.com': AngangGuo
Password for 'https://AngangGuo@github.com': [your token]
To https://github.com/AngangGuo/metrics.git
 - [deleted]         db-branch
```

### Permission Error
The following error shows out when I push to GitHub:
```
remote: Permission to AngangGuo/blog.git denied to renewalfamily.
fatal: unable to access 'https://github.com/AngangGuo/blog.git/': The requested URL returned error: 403
```

Solution
* Control Panel -> User Accounts -> Manage your credentials -> Windows Credentials
* Under Generic Credentials there are some credentials related to GitHub, Click on them and click "Remove".
* Push again and login to GitHub

### How to move a file into a folder?
* Click the pencil icon to edit the file
* In the filename field, type in the path you want the file to move in. Or
* If you want to move the file to its parent folder, type `../` before the file name to move it up one directory level.

By using commands:
```
// move readme.md into myfolder, or you can rename file using the same command
git mv readme.md myfolder

// or
mv readme.md myfolder
git add myfolder/readme.md
git rm readme.md
```
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
GitHub will automatically convert it into shortened links to the commit.

```
// for commit
Commit URL: https://github.com/jlord/sheetsee.js/commit/a5c3785ed8d6a35868bc169f07e40e889087fd2e =>	a5c3785
SHA: a5c3785ed8d6a35868bc169f07e40e889087fd2e => a5c3785

// for issue and pull request
Issue or pull request URL: https://github.com/AngangGuo/playiq/issues/4 => #4
# and issue or pull request number: #4 => #4
```

See [Autolinked references and URLs](https://docs.github.com/en/github/writing-on-github/autolinked-references-and-urls)

### How to add images to GitHub comments?
* Using Snipping Tool in Windows to get the image, copy to clipboard.
* Click on the right position in Github issue comments, paste the image in the comment directly

Or using drag and drop also works.

### How to mark GitHub issue as `bug`?
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

### How to view all my created issues?
You may create issues in many repositories. To list all the issues you created:
* Login to GitHub
* At the top of the page, click `Issues` or `Pull requests` to list all your issues/pull requests.

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