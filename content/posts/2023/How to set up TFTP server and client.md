---
title: "How to Set Up TFTP Server and Client"
date: 2023-01-01T22:46:41-08:00
categories:
  - Tech
  - Server
tags:
  - TFTP
  - Application
draft: false
---

# What is TFTP?
TFTP (Trivial File Transfer Protocol) is a simple file transferring mechanism developed as a “lighter” version of FTP.

* Instead of using the full TCP implementation, TFTP relies on the connectionless and simple UDP transport over port 69.
* TFTP only allows unidirectional file transferring.
* The TFTP server sends a block of data and waits for the acknowledgment before sending the next one.
* It provides zero control and has low overhead.
* TFTP uses a client/server communication model.

The original idea of creating TFTP was to provide booting for disk-less computers or workstations that didn’t have enough memory or disk.
These disk-less workstations usually do not have access to the full TCP/IP stack, so they need to obtain configuration information such as DHCP or BOOTP from another server.

## TFTP Server (Tftpd64) Setup
### How to install TFTP Server?
* Go to [Tftpd64](https://pjo2.github.io/tftpd64/)
* Download `Tftpd64-4.64-setup.exe` and install it

### Tftpd64 server settings
* Global tab: Select `TFTP Server` only
* TFTP tab:
  * Base Directory: `D:\MyServerFolder`
  * TFTP Security: `None`
  * Select `Bind TFTP to this address`: One of the NIC IP address
* Click `OK` and re-open the program

The TFTP Server is ready to use. 

### Server Checklist
1. The TFTP server service has to be up and the application is running.
2. Configure the right TFTP folder.
3. Make sure no Firewall and Antivirus is blocking the application and connection.
4. If you are file sharing to a remote location, don’t forget about port forwarding.
5. Make sure all your TFTP clients can reach your TFTP server.

## TFTP Client
### Turn TFTP features on in Windows 10
Search > `Turn Windows features on and off` > Select `TFTP Client` > OK

### Windows TFTP Commands
```
PS C:\Users\angan> tftp /?

Transfers files to and from a remote computer running the TFTP service.

TFTP [-i] host [GET | PUT] source [destination]

  -i              Specifies binary image transfer mode (also called
                  octet). In binary image mode the file is moved
                  literally, byte by byte. Use this mode when
                  transferring binary files.
  host            Specifies the local or remote host.
  GET             Transfers the file destination on the remote host to
                  the file source on the local host.
  PUT             Transfers the file source on the local host to
                  the file destination on the remote host.
  source          Specifies the file to transfer.
  destination     Specifies where to transfer the file.
```

Copy file example:
```
PS C:\Users\angan\Documents> tftp 192.168.0.32 get tftpd32.ini aa.ini
Transfer successful: 694 bytes in 1 second(s), 694 bytes/s
```

### Server logs

Client command #1
```
tftp -i 192.168.0.32 put .\20220710.mp4 a.mp4
^C
```
Server log #1
```
Connection received from 192.168.0.22 on port 57458 [01/01 23:47:12.787]
Write request for file <a.mp4>. Mode octet [01/01 23:47:12.788]
File <a.mp4> : error 5 in system call CreateFile Access is denied. [01/01 23:47:12.790]
Connection received from 192.168.0.22 on port 65109 [01/01 23:49:38.909]
Write request for file <a.mp4>. Mode octet [01/01 23:49:38.911]
Using local port 59206 [01/01 23:49:38.911]
TIMEOUT while waiting for Data block 22744, file <a.mp4> [01/01 23:52:52.901]
```

Client command #2
```
PS C:\Andrew\bin> tftp -i 192.168.0.32 put dl.exe
Transfer successful: 8170320 bytes in 128 second(s), 63830 bytes/s
```
Server log #2
```
Connection received from 192.168.0.22 on port 63913 [01/01 23:53:32.237]
Write request for file <dl.exe>. Mode octet [01/01 23:53:32.238]
Using local port 58286 [01/01 23:53:32.239]
<dl.exe>: rcvd 15958 blks, 8170320 bytes in 128 s. 0 blk resent [01/01 23:55:40.179]
```

## Firewall Settings
Make sure to set up the firewall rules for both server and client machine to enable TFTP server and client communication.

### TFTP Client file
TFTP is considered an unsafe protocol, so Windows does not allow it by default. 
You will have to either turn off the Windows firewall (which is not recommended) or 
add an exception on the Firewall for the TFTP Client.

To enable TFTP client access:
* Search > `Allow an app through Windows Firewall` > 
* Click `Change settings` 
* Click `Allow another app...`
* Select `C:\Windows\System32\TFTP.EXE`

Make sure the `Trivial File Transfer Protocol App` is selected in the `Allowed apps and features` column.
Enable both `Private` and `Public`(option) network access.

### Tftpd64 Server file
If you don't enable the firewall access when running the server the first time, 
then you can add the firewall rules manually similar to the above for the server file
`C:\Program Files\Tftpd64\tftpd64.exe`
