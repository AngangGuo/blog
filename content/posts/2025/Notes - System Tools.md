---
title: "Notes - System Tools"
date: 2025-01-29T13:00:56-08:00
categories:
  - Tech
  - Tools
tags:
  - Repair
  - Diskpart
  - Ventoy
  - Clonezilla
  - OOBE
  - Reset
draft: false
---

## Tools
* [Barcode Generator](https://barcode.tec-it.com/en)
* [Open Hardware Monitor](https://openhardwaremonitor.org/)
* [TechPowerUp GPU-Z](https://www.techpowerup.com/gpuz/)
* 
### Ventoy
[Ventoy](https://www.ventoy.net/en/index.html) is an open source tool to create bootable USB drive for ISO/WIM/IMG/VHD(x)/EFI files.

Download and copy Windows 11 ISO to the Ventoy partition.

### Why I can't find the HDD listed?
After booting Windows 11 installation, there's no HDD shown out.
1. You may need to disable the `Secure Boot` from BIOS
2. You may need to disable the [Intel VMD]({{% ref "#vmd" %}}) from BIOS

### Clonezilla
[Clonezilla](https://clonezilla.org/) is a partition and disk imaging/cloning program.
It helps you to do system deployment, bare metal backup and recovery.

Clonezilla live is suitable for single machine backup and restore. 
See [instruction](https://clonezilla.org/clonezilla-live-doc.php).

**Warning:**
To get the Windows image, you need to enter to the OOBE mode and 
select "Generate" for system to remove the device specific information from the image.



## Apps
### Outbyte Driver Updater
Download and automatically install Windows 10-11 drivers, and get the latest updates for your devices with [Outbyte Driver Updater](https://outbyte.com/wiki-drivers/en/du-101/). 

I re-installed the Windows system for Asus ROG laptop and downloaded/installed all the drivers from their website,
there're several device showed question marks beside them.

After I updated to the latest Windows 11, still there're two devices with question marks.
I downloaded/installed the `Outbyte Driver Updater` and it automatically updated and fixed all the drivers.

## Windows
Win + X: Show list of useful tasks (Disk Management, Device Manager, etc.)

// todo
// secure boot
// intel VMD

### How to reset forgotten Windows password?
* Boot to Windows installation screen by using a bootable USB, 
* Select `Repair your computer`
* Select `Troubleshoot` > `Command Prompt`
* Enter the following commands

Note: See [here]({{% ref "#vmd" %}}) if you can't find the HDD

```
c:
cd Windows\System32
ren Utilman.exe Utilman.exe.bak
copy cmd.exe Utilman.exe
```
Boot into Windows login page and click the `Ease of Access` icon(like a dial clock icon) to open the cmd window

Enter the following commands to reset the password:
```
// list current users in the system
net user
// change the password for a user
net user OldUserName NewPassword
// Or add a new user
net user NewUserName NewPassword /add
// Or add the user to the admin group
net localgroup administrators username /add

```
Exit and reboot into your Windows system, login using the new password.

After you reset the password
```
copy Utilman.exe.bak Utilman.exe
del Utilman.exe.bak
```

## PC

## Bitlocker
You can't restore or reset Windows to their original status if you don't have the Bitlocker keys.

### Where to find my BitLocker key?
You can find your Bitlocker recovery keys from https://account.microsoft.com/devices-list.

### How to install Windows on Bitlocker encrypted drive? {#re-install}
Boot to Windows installation disk, remove the encrypted partition and then create a new partition from the space,
install Windows to the new partition.

After finish the installation, you need to enter the [audit mode](https://blog.angang.ca/posts/2021/windows-oobe/) to install all the device drivers.
You can download the driver from the vendor website.

## Diskpart
### How to clear the encrypted HDD?
Warning:
* This will clean the whole disk. 
* If you want to keep the other volume(system restore) and only remove the encrypted C drive, 
you can delete/create the partition and install Windows 11 on this partition. See [here]({{% ref "#re-install" %}})
```
diskpart
list disk
select disk n(0, 1, ...)
list volume
clean
create part primary
active
format FS=ntfs label=C quick
format FS=ntfs label=surface quick
assign letter=C
exit
```

### How to delete a system partition?
```
diskpart
list disk
select disk 2
list partition

// select the system partition
select partition 2

// use override to remove it
// note: no partition number when delete
delete partition override
exit
```

### How to fix hard drive not show out? {#vmd}
For the newer PCs most likely you will need an intel VMD driver. 
Or disable VMD from BIOS. 

```
manage-bde -protectors -get d
```

See [Disable Intel VMD technology](https://www.asus.com/ca-en/support/faq/1044458/)