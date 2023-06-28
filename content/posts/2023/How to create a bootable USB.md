---
title: "How to Create a Bootable USB"
date: 2023-06-27T21:23:27-07:00
categories:
  - Tech
tags:
  - Bootable Disk
  - Ventoy
draft: false
---

[Ventoy](https://www.ventoy.net/en/index.html) is an open source tool to create bootable USB drive for ISO/WIM/IMG/VHD(x)/EFI files.
With ventoy, you don't need to format the disk over and over, you just need to copy the ISO/WIM/IMG/VHD(x)/EFI files to the USB drive and boot them directly.



x86 Legacy BIOS, IA32 UEFI, x86_64 UEFI, ARM64 UEFI and MIPS64EL UEFI are supported in the same way.

## Creating a Bootable USB
### Install Ventoy
* [Download]() the installation package, like ventoy-x.x.xx-windows.zip and decompress it.
* Run `Ventoy2Disk.exe` , select the USB device and click Install or Update button.

Note:
After the installation is complete, the USB drive will be divided into 2 partitions.
The 1st partition was formatted with exFAT filesystem.

### Copy Image Files
* Download ISO or other supported image files from vendor website. 
e.g. [Download Windows 11 Disk Image - ISO](https://www.microsoft.com/en-gb/software-download/windows11)
* Copy iso files to the 1st partition of this Ventoy USB disk. 

Note:
* You can also browse ISO/WIM/IMG/VHD(x)/EFI files in local disks and boot them.
* You can place the image files anywhere of the 1st partition. 
Ventoy will search all the directories and subdirectories recursively to find all the image files and list them in the boot menu alphabetically. Also you use plugin configuration to tell Ventoy only to search for image files in a fixed directory (and its subdirectories).

### Secure Boot Problem
See [here](https://www.ventoy.net/en/doc_secure.html)

In my case, I need to disable Secure Boot in BIOS. 
`Shift+Restart` to enter BIOS setup.