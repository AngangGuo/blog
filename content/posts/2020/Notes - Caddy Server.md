---
title: "Notes - Caddy Server"
date: 2020-09-30T21:55:29-07:00
categories:
 - Tech
 - Server
tags:
 - Caddy
 - Go
draft: false
---

## How to install Caddy in Ubunbu?
```
$ echo "deb [trusted=yes] https://apt.fury.io/caddy/ /" \
    | sudo tee -a /etc/apt/sources.list.d/caddy-fury.list
$ sudo apt update
$ sudo apt install caddy

andrew@svr:~$ sudo apt install caddy
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following NEW packages will be installed:
  caddy
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 11.5 MB of archives.
After this operation, 33.1 MB of additional disk space will be used.
Get:1 https://apt.fury.io/caddy  caddy 2.2.0 [11.5 MB]
Fetched 11.5 MB in 12s (926 kB/s)                                                                                                      
Selecting previously unselected package caddy.
(Reading database ... 78985 files and directories currently installed.)
Preparing to unpack .../archives/caddy_2.2.0_amd64.deb ...
Unpacking caddy (2.2.0) ...
Setting up caddy (2.2.0) ...
Created symlink /etc/systemd/system/multi-user.target.wants/caddy.service 鈫/lib/systemd/system/caddy.service.
andrew@svr:~$ 
```

## Original `/etc/caddy/Caddyfile`
```
# The Caddyfile is an easy way to configure your Caddy web server.
#
# Unless the file starts with a global options block, the first
# uncommented line is always the address of your site.
#
# To use your own domain name (with automatic HTTPS), first make
# sure your domain's A/AAAA DNS records are properly pointed to
# this machine's public IP, then replace the line below with your
# domain name.
:80

# Set this path to your site's directory.
root * /usr/share/caddy

# Enable the static file server.
file_server

# Another common task is to set up a reverse proxy:
# reverse_proxy localhost:8080

# Or serve a PHP site through php-fpm:
# php_fastcgi localhost:9000

# Refer to the Caddy docs for more information:
# https://caddyserver.com/docs/caddyfile
```

## Steps to run my server:
* Change configuration in `etc/caddy/Caddyfile`
```
angang.ca {
     reverse_proxy localhost:8001
}

www.angang.ca {
  redir https://angang.ca{uri}
}
```

* Reload Caddy server
```
caddy reload
```

* Run Shareverses
```
andrew@svr:~/go/src/shareverses$ sudo -b ./shareverses -dev > log.txt
```

## Trouble Shooting
### Administration Error
```
andrew@svr:~/main$ caddy start
2020/10/01 05:27:45.023 INFO    using adjacent Caddyfile
run: loading initial config: loading new config: starting caddy administration endpoint: listen tcp 127.0.0.1:2019: bind: address already in use
start: caddy process exited with error: exit status 1
```

You're looking for the admin global option to change which port/address Caddy uses for the admin endpoint. You can also turn it off entirely if you don't need it.
```
{
    admin off
}

... rest of your Caddyfile
```

### Permission
See https://github.com/caddyserver/caddy/issues/3234

If you turn off the admin endpoint with the admin off global option, you're also turning off the ability to reload configuration.
```
Caddy Service specify in the doc :

# This service file requires the following:
#
# 1) Group named caddy:
#      $ groupadd --system caddy
#
# 2) User named caddy, with a writeable home folder:
#      $ useradd --system \
#           --gid caddy \
#           --create-home \
#           --home-dir /var/lib/caddy \
#           --shell /usr/sbin/nologin \
#           --comment "Caddy web server" \
#           caddy
#
# 3) Caddyfile at /etc/caddy/Caddyfile that is
#    readable by the caddy user
#

[Unit]
Description=Caddy Web Server
Documentation=https://caddyserver.com/docs/
After=network.target

[Service]
User=caddy
Group=caddy
ExecStart=/usr/bin/caddy run --config /etc/caddy/Caddyfile --resume --environ
ExecReload=/usr/bin/caddy reload --config /etc/caddy/Caddyfile
TimeoutStopSec=5s
LimitNOFILE=1048576
LimitNPROC=512
PrivateTmp=true
ProtectSystem=full
AmbientCapabilities=CAP_NET_BIND_SERVICE

[Install]
WantedBy=multi-user.target
```