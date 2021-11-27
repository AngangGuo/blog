---
title: "Windows Installation - Can't Find Hard Drives"
date: 2021-11-25T11:59:42-08:00
categories:
  - Tech
  - Work
tags:
  - Windows
  - Driver
draft: false
---

We have an Asus UX325EA-DS51 laptop that failed to start up. 
The Windows operating system need to be re-installed.

### Problem
I can see the hard drive in the BIOS.
But during Windows installation process, the hard drive can't be found by the installation program.

### Solution
Go to Intel website and download the `Intel Rapid Storage Technology (Intel RST) Driver 18.6.1.1016` which supports 10th Gen and 11th Gen Intel Core platforms. 
File name is [F6flpy-x64 - VMD.zip](https://www.intel.com/content/www/us/en/download/19512/intel-rapid-storage-technology-driver-installation-software-with-intel-optane-memory-10th-and-11th-gen-platforms.html)

Extract the file into a folder in the installation disk then boot from the installation USB disk. 

When prompting `"Where do you want to install Windows?"`, 
* Select `Load driver`
* Select the driver folder (you can use mouse or the right arrow key to expand the folder)
* Install driver `Intel RST VMD Controller 9A0B(iaStorVD.inf)`
* Your SSD driver will show out
* (Option)Delete the old partitions and create new partitions
* Select the partition to install Windows

After Windows installation, you can find some devices are marked as unknown devices. When you connect to internet,
Windows will install MyASUS automatically and all the drivers will be installed.

### Windows Audit Mode and OOBE
After Windows installation finished, it will reboot to a screen shows `Let's start with region.`.
Press `Ctrl + Shift + F3` will enter the Audit mode using the built-in Administrator account
You can generate 

## Intel
### Which RST file should I download?
The Intel® Rapid Storage Technology (Intel® RST) Driver 18.6.1.1016 supports 10th Gen and 11th Gen Intel Core platforms.

There are three files you can download from [Intel Download Center](https://www.intel.com/content/www/us/en/download/19512/intel-rapid-storage-technology-driver-installation-software-with-intel-optane-memory-10th-and-11th-gen-platforms.html) 
* `SetupRST.exe`：This file is the new installer that will install the Intel RST driver and start the process of installing the Intel® Optane™ Memory and Storage Management application from the Microsoft Store*

以下软件包仅包含驱动程序文件，必须通过 "有磁盘"过程安装：
* `F6flpy-x64 - VMD.zip`：仅针对启用 VMD 的平台的新驱动程序包。驱动程序二进制文件是 `iaStorVD.sys`。
* `F6flpy-x64-Non-VMD.zip`：此驱动程序包针对未启用 VMD 或不支持 VMD 的平台。驱动程序二进制文件是 `iaStorAC.sys`。

Tips:
It's better to extract the driver to the installation USB disk. 
If you copy the file to another disk, it may cause `0x80300001` error when you change disk.

## Asus
### How to enter the BIOS?
For Asus Laptop:
Press and hold the **F2** button of the keyboard, and then press the **Power button**.
Hold **F2** button until the BIOS configuration display.

### How to enter BIOS from Windows 10?
If your computer is Windows 10, search "Change advanced startup option":
Open > Advanced startup > Restart now > Troubleshoot > Advanced Options > UEFI Firmware Settings > Restart 

## Useful Links
* [Can't find drives when installing Windows 11/10](https://www.asus.com/support/faq/1044458)
* [How to Configure Optane Memory with Intel RST on VMD Capable Platform](https://www.intel.com/content/www/us/en/support/articles/000057787/memory-and-storage/intel-optane-memory.html)
* [SSD Not Showing Up While Windows 10 Installation](https://www.youtube.com/watch?v=7nWfZh2X3ZY)
* [Asus Download Center](https://www.asus.com/support/Download-Center/)
* [Intel® Rapid Storage Technology Driver Installation Software](https://www.intel.com/content/www/us/en/download/19512/intel-rapid-storage-technology-driver-installation-software-with-intel-optane-memory-10th-and-11th-gen-platforms.html)
* [如何在支持云的平台上英特尔® 傲腾™ RAID 英特尔® RST内存英特尔® VMD内存](https://www.intel.cn/content/www/cn/zh/support/articles/000057787/memory-and-storage/intel-optane-memory.html)