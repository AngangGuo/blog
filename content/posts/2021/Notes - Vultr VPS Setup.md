---
title: "Notes   Vultr VPS Setup"
date: 2021-10-12T12:35:39-07:00
draft: true
---


## Login
[PuTTY](https://www.putty.org/) or [MobaXterm](https://mobaxterm.mobatek.net/download.html)

MobaXterm > New SSH Session(get this login information from your Vultr console):
Port: 22
IP: xxx.xxx.xxx.xxx
Username: root
Password: xxxx

// add a new user
$ adduser andrew
// sudo permission
$ usermod -aG sudo andrew
// switch to your new user
$ su - andrew

## Setup
$ sudo apt update
$ sudo apt upgrade
$ sudo apt-get dist-upgrade
$ sudo reboot

// time zone
$ sudo dpkg-reconfigure tzdata

Current default time zone: 'America/Vancouver'
Local time is now:      Tue Oct 12 18:08:05 PDT 2021.
Universal Time is now:  Wed Oct 13 01:08:05 UTC 2021.

// auto remove
$ sudo apt autoremove -y

## Firewall
You can set the firewall from Vultr or from within your server by using `ufw`.
See https://www.vultr.com/docs/vultr-firewall
See https://ubuntu.com/server/docs/security-firewall
https://www.vultr.com/docs/initial-secure-server-configuration-of-ubuntu-18-04
https://www.vultr.com/docs/configure-ubuntu-firewall-ufw-on-ubuntu-18-04

### UFW
ufw status
ufw enable
ufw allow https
ufw allow http
ufw allow ssh

Settings > Firewall > Add Firewall Group > "MyFirewallGroup" > Add Firewall Rule

## Tips
* MobaXterm: right click to paste, shift + right click to show context menu
* 