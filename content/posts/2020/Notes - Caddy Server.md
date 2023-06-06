---
title: "Notes - Caddy Server"
date: 2020-09-30T21:55:29-07:00
categories:
 - Tech
 - Application
tags:
 - Caddy
 - Go
 - Server
draft: false
---

### Installation
### How to install Caddy in Ubunbu?
* Commands list
```
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo tee /etc/apt/trusted.gpg.d/caddy-stable.asc
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update

andrew@koreasvr:~$ sudo apt install caddy
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following NEW packages will be installed:
  caddy
0 upgraded, 1 newly installed, 0 to remove and 2 not upgraded.
Need to get 11.6 MB of archives.
After this operation, 33.6 MB of additional disk space will be used.
Get:1 https://dl.cloudsmith.io/public/caddy/stable/deb/debian any-version/main amd64 caddy amd64 2.4.5 [11.6 MB]
Fetched 11.6 MB in 2s (5,928 kB/s)
Selecting previously unselected package caddy.
(Reading database ... 107417 files and directories currently installed.)
Preparing to unpack .../archives/caddy_2.4.5_amd64.deb ...
Unpacking caddy (2.4.5) ...
Setting up caddy (2.4.5) ...
Created symlink /etc/systemd/system/multi-user.target.wants/caddy.service → /lib/systemd/system/caddy.service.

```

### Verification
* Service
```
$ sudo systemctl status caddy
● caddy.service - Caddy
     Loaded: loaded (/lib/systemd/system/caddy.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2021-10-12 18:56:56 PDT; 13h ago
       Docs: https://caddyserver.com/docs/
    Process: 3563 ExecReload=/usr/bin/caddy reload --config /etc/caddy/Caddyfile (code=exited, status=0/SUCCESS)
   Main PID: 2051 (caddy)
      Tasks: 7 (limit: 1071)
     Memory: 10.5M
     CGroup: /system.slice/caddy.service
             └─2051 /usr/bin/caddy run --environ --config /etc/caddy/Caddyfile

Oct 12 23:14:57 koreasvr caddy[2051]: {"level":"info","ts":1634105697.454619,"logger":"tls.issuance.acme","msg":"done waiting on internal rate limiter","identifiers">
Oct 12 23:15:29 koreasvr caddy[2051]: {"level":"info","ts":1634105729.7737117,"logger":"tls.obtain","msg":"certificate obtained successfully","identifier":"xingfu.br>
Oct 12 23:15:29 koreasvr caddy[2051]: {"level":"info","ts":1634105729.7738106,"logger":"tls.obtain","msg":"releasing lock","identifier":"xingfu.brideofchrist.ca"}
lines 1-21/21 (END)

```
* Show default configuration
```
$ curl localhost:2019/config/

{
   "apps":{
      "http":{
         "servers":{
            "srv0":{
               "listen":[
                  ":80"
               ],
               "routes":[
                  {
                     "handle":[
                        {
                           "handler":"vars",
                           "root":"/usr/share/caddy"
                        },
                        {
                           "handler":"file_server",
                           "hide":[
                              "/etc/caddy/Caddyfile"
                           ]
                        }
                     ]
                  }
               ]
            }
         }
      }
   }
}
```

* You will see the following message by visiting your host IP address: `http://xxx.xxx.xxx.xxx/`
```
Congratulations! おめでとう! Felicidades! 恭喜! बधाई हो! Поздравляю!  🎊
Your web server is working. Now make it work for you. 💪

Caddy is ready to serve your site over HTTPS:

Point your domain's A/AAAA DNS records at this machine.
Upload your site's files to /var/www/html.
Edit your Caddyfile at /etc/caddy/Caddyfile:
Replace :80 with your domain name
Change the site root to /var/www/html
Reload the configuration: systemctl reload caddy
Visit your site!

If that didn't work 😶
It's okay, you can fix it! First check the following things:

Service status: systemctl status caddy
Logs: journalctl --no-pager -u caddy
Are your site's files readable by the caddy user and group? ls -la /var/www/html
Is the caddy home directory writeable? ls -la /var/lib/caddy
Ensure your domain's A and/or AAAA records point to your machine's public IP address: dig example.com
Are your ports 80 and 443 externally reachable, and is Caddy able to bind to them? Check your firewalls, port forwarding, and other network configuration.
```

## Configuration
### Simple command list
* Point your domain's A/AAAA DNS records at this machine. See below on "How to set up your domain?" section.
* Upload your site's files to `/var/www/html`. MobaXterm SFTP client can't show Chinese path correctly, prefer to use FileZilla to upload files, 
* Edit your Caddyfile at /etc/caddy/Caddyfile:
  * Replace `:80` with your domain name
  * Change the site root to `/var/www/html`
* Reload the configuration: `systemctl reload caddy`
* Visit your site!

If you can't load your file into `/var/www/html` because of permission error, using the following commands to fix it:
```
mkdir /var/www/html
sudo chown -R andrew:caddy /var/www/html
```

Here is a simple working `Caddyfile` example:
```
// example of /etc/caddy/Caddyfile
blog.angang.ca {
    # Set this path to your site's directory.
    root * /var/caddy/blog.angang.ca

    # Enable the static file server.
    file_server
}
```

### How to set up your domain?
* Login to https://canspace.ca/
* Domains > Manage DNS > DNS Zone > Click `Edit Zone` near your domain name
* Click `Add Record`:
```
// A record
Name: @
Type: A
TTL: 14400
RDATA: (server IP)
```

```
Name: blog (sub domain)
Type: CNAME
TTL: 14400
RDATA: angang.ca (damain name)
```

### Default configuration of `/lib/systemd/system/caddy.service`
```
# caddy.service
#
# For using Caddy with a config file.
#
# Make sure the ExecStart and ExecReload commands are correct
# for your installation.
#
# See https://caddyserver.com/docs/install for instructions.
#
# WARNING: This service does not use the --resume flag, so if you
# use the API to make changes, they will be overwritten by the
# Caddyfile next time the service is restarted. If you intend to
# use Caddy's API to configure it, add the --resume flag to the
# `caddy run` command or use the caddy-api.service file instead.

[Unit]
Description=Caddy
Documentation=https://caddyserver.com/docs/
After=network.target network-online.target
Requires=network-online.target

[Service]
Type=notify
User=caddy
Group=caddy
ExecStart=/usr/bin/caddy run --environ --config /etc/caddy/Caddyfile
ExecReload=/usr/bin/caddy reload --config /etc/caddy/Caddyfile
TimeoutStopSec=5s
LimitNOFILE=1048576
LimitNPROC=512
PrivateTmp=true
ProtectSystem=full
AmbientCapabilities=CAP_NET_BIND_SERVICE

[Install]
WantedBy=multi-user.target
~

```

### Default configuration of `/etc/caddy/Caddyfile`
```
# The Caddyfile is an easy way to configure your Caddy web server.
#
# Unless the file starts with a global options block, the first
# uncommented line is always the address of your site.
#
# To use your own domain name (with automatic HTTPS), first make
# sure your domain's A/AAAA DNS records are properly pointed to
# this machine's public IP, then replace ":80" below with your
# domain name.

:80 {
        # Set this path to your site's directory.
        root * /usr/share/caddy

        # Enable the static file server.
        file_server

        # Another common task is to set up a reverse proxy:
        # reverse_proxy localhost:8080

        # Or serve a PHP site through php-fpm:
        # php_fastcgi localhost:9000
}

# Refer to the Caddy docs for more information:
# https://caddyserver.com/docs/caddyfile

```


## Tips
### How to redirect sub-domain?
```
angang.ca {
     reverse_proxy localhost:8001
}

www.angang.ca {
  redir https://angang.ca{uri}
}
```
### How to use Caddy service?
```
// systemctl start starts (activates) the service immediately
sudo systemctl start caddy
// after you modified the /etc/caddy/Caddyfile
sudo systemctl restart caddy
// systemctl enable configures the system to start the service at next reboot 
sudo systemctl enable caddy
```

### How to use Caddy binery file?
```
caddy reload
```

* Run Shareverses
```
andrew@svr:~/go/src/shareverses$ sudo -b ./shareverses -dev > log.txt
```

* Serve a static website(See [here](https://caddyserver.com/docs/quick-starts/static-files))
Go to the root of the static website, run the following command:
```
caddy file-server --listen :8080
```

* If you don't have index.html in the root folder, use following command to list files:
```
caddy file-server --browse
```

## Trouble Shooting
### Administration Error
```
andrew@svr:~/main$ caddy start
2020/10/01 05:27:45.023 INFO    using adjacent Caddyfile
run: loading initial config: loading new config: starting caddy administration endpoint: listen tcp 127.0.0.1:2019: bind: address already in use
start: caddy process exited with error: exit status 1
```

You're looking for the admin global option to change which port/address Caddy uses for the admin endpoint. 
You can also turn it off entirely if you don't need it.
```
{
    admin off
}

... rest of your Caddyfile
```

### Permission error
If you can't load data into `/var/www/html`, or your website can't access the folder, 
check your file/folder permission. It need to be accessed by `caddy` user or `caddy` group.
```
// not recommand
sudo chmod -R 777 /var/www/html/
// serve website by caddy
sudo chown -R caddy:caddy /var/www/html/
// if you want to load files into the folder
sudo chown -R andrew:caddy /var/www/html/
```
See [Ubuntu File Permission](https://help.ubuntu.com/community/FilePermissions)

### Reload error
See [Cannot use caddy reload if admin endpoint is turned off](https://github.com/caddyserver/caddy/issues/3234)

If you turn off the admin endpoint with the admin off global option, 
you're also turning off the ability to reload configuration.

### Caddy service file requires the following:
```
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

```