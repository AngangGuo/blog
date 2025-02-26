---
title: "Windows Installation - Can't Find Hard Drives"
date: 2021-11-25T11:59:42-08:00
categories:
  - Tech
  - Windows
tags:
  - Windows
  - VMD
  - Intel RST
draft: false
---

## HDD Won't Show Out
We have an Asus UX325EA-DS51 laptop that failed to start up. 
The Windows operating system need to be re-installed.

### Problem
I can see the hard drive in the BIOS.
But during Windows installation process, the hard drive can't be found by the installation program.

### Solution
1. Disable VMD Controller from BIOS(Advanced menu)

![bios disble vmd](/images/2021/windows-installation-disable-vmd.jpg)

2. Download the latest `Intel RST` driver and load the driver during Windows installation

Go to Intel website and download the `Intel Rapid Storage Technology (Intel RST) Driver 18.6.1` or later which supports 10th Gen and 11th Gen Intel Core platforms. 

Extract driver files from `SetupRST.exe`:
* Open terminal in the directory with SetupRST.exe by right-clicking the directory and selecting "Open in Terminal" or "Open PowerShell here"
* Enter the following command: `./SetupRST.exe -extractdrivers SetupRST_extracted`
* Copy all driver files from the SetupRST-extracted to a USB key media.
* For Microsoft Windows: During the operating system installation, after selecting the location to install 
      Windows, click `Load Driver` to install a third party SCSI or RAID driver.
* When prompted, insert the USB media and press Enter.

After Windows installation, you can find some devices are marked as unknown devices. 
See following on how to update device drivers. 

### How to update device drivers?
**Normal Boot Up**

Login to Windows and connect to internet, Windows will install MyASUS automatically and all the drivers will be installed.
If it doesn't update automatically, see below on how to update the driver from `Windows Update`

**In Audit Mode**

But if in Audit mode(`Ctrl+Shift+F3`), the driver may not update automatically. 

You can right-click the device, select `Properties` and 
update the driver from `Windows Update`(preferred); 

Or download the driver from manufacture site and install them manually.
(I installed all the downloaded Asus drivers but still lots of unknown devices)

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

### How to install device drivers?
Windows Update > Check for update > Windows will download the update and install them automatically.
If in Audit mode and there's pending restart, use command `shutdown -r -t 00` to restart computer.

Check the Device Manager, if there's any unknown device, click the Windows update again to update the Windows.
You may need to restart computer several time. Eventually all the device drivers will be installed.  

Device Manager > Other Devices > Right-click the device with warning mark > Update driver > Windows Update

**Alternative**
Download the device driver from vendor website and install them individually. (Prefer to use Windows Update)

**Warning**: 
If in Audit mode, only use command `shutdown -r -t 00` to restart computer; 
Don't click the restart button on Windows -  it'll lock the Administrator account.


### How to enter BIOS from within Windows 10?

If your computer is Windows 10, search "Change advanced startup option":
Open > Advanced startup > Restart now > Troubleshoot > Advanced Options > UEFI Firmware Settings > Restart 

## Account
To set up Windows, you may need a Microsoft account. 

We can use the testing account **rl2login@outlook.com**. The computer will be linked to this account. 
Remember to **Unlink** the device from this account after you finished testing.

### How to unlink a device from your account?
**Find System Name**:
* Open `System Infomation`
* Get and record your system name

**Unlink From Windows Account**:
* Sign In to your Microsoft account from [here](https://account.microsoft.com/account/), 
* Click `view all devices` 
* Find the device by using the device name recorded in the first step
* Click `See Detils`, verify the name
* Click `Remove this device` to unlink it from this Windows account.

## Useful Links
* [Can't find drives when installing Windows 11/10](https://www.asus.com/support/faq/1044458)
* [How to Configure Optane Memory with Intel RST on VMD Capable Platform](https://www.intel.com/content/www/us/en/support/articles/000057787/memory-and-storage/intel-optane-memory.html)
* [SSD Not Showing Up While Windows 10 Installation](https://www.youtube.com/watch?v=7nWfZh2X3ZY)
* [Asus Download Center](https://www.asus.com/support/Download-Center/)
* [Intel® Rapid Storage Technology Driver Installation Software](https://www.intel.com/content/www/us/en/download/19512/intel-rapid-storage-technology-driver-installation-software-with-intel-optane-memory-10th-and-11th-gen-platforms.html)
* [如何在支持云的平台上英特尔® 傲腾™ RAID 英特尔® RST内存英特尔® VMD内存](https://www.intel.cn/content/www/cn/zh/support/articles/000057787/memory-and-storage/intel-optane-memory.html)