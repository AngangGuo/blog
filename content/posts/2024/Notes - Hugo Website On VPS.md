---
title: "Notes - Hugo website on VPS"
date: 2024-12-17T00:19:45-08:00
categories:
  - Tech
  - Web
tags:
  - Vultr
  - Ubuntu
  - Caddy
  - Hugo
draft: false
---

Follow these steps to set up a server for my document site.

## Server Setup
### Vultr
* Go to https://my.vultr.com/
* Deploy new server > Ubuntu
* Get root password and login to server
* Create and add sudo user

### Putty
* SSH to server
* Login as sudo user

## Caddy server
### Installation
```
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https curl
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/testing/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-testing-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/testing/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-testing.list

sudo apt update
sudo apt install caddy

systemctl status caddy
```
### Caddyfile
Tips: 
* You can use `nano` or `vim` to edit file
* After you update the Caddyfile, run `systemctl reload caddy` or `systemctl daemon-reload` to reload it
* Use `*` after `basic_auth` for whole site authentication, `/` does not work

```
# nano /etc/caddy/Caddyfile
# location: /etc/caddy/Caddyfile
rlpro.angang.ca {
    root * /var/www/html
    
    # use "caddy hash-password" command to generate the password hash
    basic_auth * {
        name $2a$14$HsHZgDUA9...
    }
    
    file_server
}
```

Note: 
if you use `/home/rl/rlpro/public` folder as Caddy file server root folder, 
the client will not able to visit the page, for they don't have permission to access the page inside the user's home folder.

#### Password Hash
You can use `caddy hash-password` to generate hash password for basic-auth(or https://bcrypt.online/).

A BCrypt hash includes salt and as a result this algorithm returns different hashes for the same input

### Testing
1. Point your domain's A/AAAA DNS records at this machine.
2. Upload your site's files to /var/www/html.
3. Edit your Caddyfile at /etc/caddy/Caddyfile:
4. Replace :80 with your domain name
5. Change the site root to /var/www/html
6. Reload the configuration: systemctl reload caddy
7. Visit your site!

## DNS set up
Add an `A` record point to my server IP in my DNS server:
* Name: rlpro
* Type: A
* RDATA: 104.207.158.254

## Hugo
```
sudo snap install hugo // newer version
sudo apt install hugo // old version from Ubuntu bundle
```

### Preparing Site files
* git clone
* hugo
* cp -r

### Transfer files
You can use `FileZilla` to transfer files between server and your local machine

* Protocol: SFTP
* Host: 104.207.xxx.xxx

### Firewall
Only port 22 is allowed by default
```
andrew@rlpro:~/rl$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere
22/tcp (v6)                ALLOW       Anywhere (v6)
```

You need to enable port 80(HTTP) and port 443(HTTPS) for Caddy to work.
```
andrew@rlpro:~/rl$ sudo ufw allow http
andrew@rlpro:~/rl$ sudo ufw allow https
andrew@rlpro:~/rl$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere
80                         ALLOW       Anywhere
443                        ALLOW       Anywhere
22/tcp (v6)                ALLOW       Anywhere (v6)
80 (v6)                    ALLOW       Anywhere (v6)
443 (v6)                   ALLOW       Anywhere (v6)
```

## Trubleshooting
### Caddy failed to load Caddyfile
After I added the basic_auth and reload, it shows the following error message:
```

```

### Caddy failed to start
Caddy doesn't work. After I run the command `sudo systemctl status caddy`, it shows the following error message:

Error message: `challenge failed: Timeout during connect (likely firewall problem)`

This error is caused by firewall problem. See above Firewall section for details.
```
andrew@rlpro:~/rl/rlpro$ sudo systemctl status caddy
● caddy.service - Caddy
     Loaded: loaded (/usr/lib/systemd/system/caddy.service; enabled; preset: enabled)
     Active: active (running) since Tue 2024-12-17 07:35:28 UTC; 26min ago
       Docs: https://caddyserver.com/docs/
    Process: 1479 ExecReload=/usr/bin/caddy reload --config /etc/caddy/Caddyfile --force (code=exited, status=0/SUCCESS)
   Main PID: 897 (caddy)
      Tasks: 7 (limit: 1061)
     Memory: 47.6M (peak: 57.9M)
        CPU: 594ms
     CGroup: /system.slice/caddy.service
             └─897 /usr/bin/caddy run --environ --config /etc/caddy/Caddyfile

Dec 17 08:04:58 rlpro caddy[897]: {"level":"info","ts":1734422698.487651,"logger":"tls.obtain","msg":"obtaining certificate","identifier":"rlpro.angang.ca"}
Dec 17 08:04:58 rlpro caddy[897]: {"level":"info","ts":1734422698.4903393,"logger":"http","msg":"using ACME account","account_id":"https://acme-staging-v02.api.letsencrypt.org/acme/acct/176418624","account_contact":[]}
Dec 17 08:04:58 rlpro caddy[897]: {"level":"info","ts":1734422698.6360738,"logger":"http.acme_client","msg":"trying to solve challenge","identifier":"rlpro.angang.ca","challenge_type":"http-01","ca":"https://acme-staging-v02.api.letsencrypt.org/directory"}
Dec 17 08:05:08 rlpro caddy[897]: {"level":"error","ts":1734422708.8593903,"logger":"http.acme_client","msg":"challenge failed","identifier":"rlpro.angang.ca","challenge_type":"http-01","problem":{"type":"urn:ietf:params:acme:error:connection","title":"","detail":"104.207.158.254: Fetching http://rlpro.angang.ca/.well-known/acme-challenge/QrX_5EOWRBwpADFgW7slDmkuYrMrlbVou1OUvAd_eKE: Timeout during connect (likely firewall problem)","instance":"","subproblems":[]}}
Dec 17 08:05:08 rlpro caddy[897]: {"level":"error","ts":1734422708.8594453,"logger":"http.acme_client","msg":"validating authorization","identifier":"rlpro.angang.ca","problem":{"type":"urn:ietf:params:acme:error:connection","title":"","detail":"104.207.158.254: Fetching http://rlpro.angang.ca/.well-known/acme-challenge/QrX_5EOWRBwpADFgW7slDmkuYrMrlbVou1OUvAd_eKE: Timeout during connect (likely firewall problem)","instance":"","subproblems":[]},"order":"https://acme-staging-v02.api.letsencrypt.org/acme/order/176418624/21358375304","attempt":1,"max_attempts":3}
Dec 17 08:05:20 rlpro caddy[897]: {"level":"error","ts":1734422720.1929471,"logger":"tls.obtain","msg":"could not get certificate from issuer","identifier":"rlpro.angang.ca","issuer":"acme-v02.api.letsencrypt.org-directory","error":"HTTP 400 urn:ietf:params:acme:error:connection - 104.207.158.254: Timeout during connect (likely firewall problem)"}
Dec 17 08:05:20 rlpro caddy[897]: {"level":"error","ts":1734422720.1929915,"logger":"tls.obtain","msg":"will retry","error":"[rlpro.angang.ca] Obtain: [rlpro.angang.ca] solving challenge: rlpro.angang.ca: [rlpro.angang.ca] authorization failed: HTTP 400 urn:ietf:params:acme:error:connection - 104.207.158.254: Timeout during connect (likely firewall problem) (ca=https://acme-staging-v02.api.letsencrypt.org/directory)","attempt":3,"retrying_in":120,"elapsed":246.333845973,"max_duration":2592000}

```