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

## Common Commands
### How to show file permission?
```
ls /var/www -al
```
### How to add a user?
```
sudo adduser new_username
// or
sudo useradd new_username
```
### How to remove a user?
```
sudo userdel username
// remove user's home folder
sudo rm -r /home/username
```
### How to change other user's password?
```
// set / change password for user postgres
sudo passwd postgres
```

### How to add a user to sudo group?
```
adduser username sudo
// or
usermod -aG sudo username
```

### How to execute commands as other user?
```
su // as root
su - postgres // login as postgres
su postgres // switch to postgres

// preserve the entire environment (HOME, SHELL, USER, and LOGNAME) of the calling user
su -p postgres 
```

### How to change file permission?
See https://help.ubuntu.com/community/FilePermissions

1. By changing file ownership?
```
cp ~/.ssh/authorized_keys ~[username]/.ssh/authorized_keys
chown -R [username]:[username] ~[username]/.ssh
```
2. By changing file permission
```
sudo chmod -R 777 /var/www/html/
```
### How to list all users?
```
cat /etc/passwd
// or
cut -d: -f1 /etc/passwd
```

## Ubuntu
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

### How to use `systemctl`?
```
systemctl daemon-reload
systemctl status caddy
// systemctl start starts (activates) the service immediately
systemctl start caddy
systemctl restart caddy
// systemctl enable configures the system to start the service at next reboot
systemctl enable caddy
```

### How to upgrade from Ubuntu Linux 16.04 to 18.04
```
$ sudo apt update
$ sudo apt upgrade
$ sudo reboot

// optional 1: open port 1022
// $ sudo ufw allow 1022/tcp comment 'Temp open port ssh tcp port 1022 for upgrade'
// optional 2: or temporate disable filewall
// $ sudo ufw status
// $ sudo ufw disable
// $ sudo ufw enable

$ sudo do-release-upgrade

// verify
$ uname -r

// optional: remove port 1022
// $ sudo ufw delete allow 1022/tcp

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

### How to run donation program?
```
chmod +x ./donation
sudo -b ./donation > log.txt
// to stop it if it is running
ps -eaf | grep donation
// kill it by the process id xxx
sudo kill xxx
```  
