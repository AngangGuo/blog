---
title: "Notes - Linux"
date: 2020-07-14T10:07:09-07:00
categories:
 - Tech
 - OS
tags:
 - Ubuntu
draft: false
---

## Linux Notes

### How to show ubuntu version?
Press `Ctrl + Alt + T` open terminal and type the following command:
```
$ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description: Ubuntu 19.04
Release: 19.04
Codename: disco
```

### Why repository no longer has a Release file?
When I run `sudo apt-get update` command, it shows:
```
E: The repository 'http://security.ubuntu.com/ubuntu disco-security Release' no longer has a Release file.
N: Updating from such a repository can't be done securely, and is therefore disabled by default.
N: See apt-secure(8) manpage for repository creation and user configuration details.
```
**Reason:**<br>
Ubuntu 19.04 is pass end-of-life and the release files are removed from the repository

**Solution:**<br>
Change all of your apt-sources to "old-releases.ubuntu.com". 
This sed command works with any/all ubuntu.com URLs and saves a backup copy in case you need to revert:
```
sudo sed -i.save -e 's/\/\/.*ubuntu.com/\/\/old-releases.ubuntu.com/g' /etc/apt/sources.list
```
Upgrade all packages in your current (old and outdated) release and 
make sure you have the update manager installed:
```
sudo apt-get update
sudo apt update && sudo apt full-upgrade -y
//sudo apt-get dist-upgrade
//sudo apt-get install update-manager-core
sudo reboot
// Upgrade to the latest release:
sudo do-release-upgrade
```
  
