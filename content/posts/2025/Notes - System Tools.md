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
draft: false
---

## Ventoy
[Ventoy](https://www.ventoy.net/en/index.html) is an open source tool to create bootable USB drive for ISO/WIM/IMG/VHD(x)/EFI files.

## Windows
Win + X: Show list of useful tasks (Disk Management, Device Manager, etc.)

### Keys to enter BIOS
* Acer: F12, F9 or Esc
* Asus: Esc or F8
* Compaq: F9 or Esc
* Dell: F12
* HP: F9 or Esc
* Lenovo: F12, F8, F10, F11
* MSI: F11
* Razer Blade: F12
* Samsung: F12 or F2
* Sony: Esc or F11
* Toshiba: F12
* Republic of Games: F2 or F9

### Where to find my BitLocker key?
You can find your Bitlocker recovery keys from https://account.microsoft.com/devices-list.

### How to install Windows on Bitlocker encrypted drive?
Boot

## Diskpart
### How to clear the encrypted HDD?
Note: 
* This will clean the whole disk. 
* If you want to keep the other volume(system restore) and only remove the encrypted C drive, 
you can delete/create the partition and install Windows 11 on this partition.
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

### How to fix hard drive not show out?
For the newer PCs most likely you will need an intel VMD driver. 
Or disable VMD from BIOS. 

```
manage-bde -protectors -get d
```
