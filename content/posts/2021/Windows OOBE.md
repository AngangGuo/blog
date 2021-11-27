---
title: "Windows Audit Mode or OOBE"
date: 2021-11-26T14:48:33-08:00
draft: true
---

### Audit Mode
Enter audit mode and install drivers

### Prevent Sysprep from removing installed devices
When you set up a Windows PC, Windows Setup configures all detected devices. 
Generalizing a Windows installation uninstalls these configured devices, but doesn't remove device drivers from the PC.

If you're deploying an image to computers that have identical hardware and devices as the original PC, 
you can keep devices installed on the computer during system generalization by using an unattend file with 
Microsoft-Windows-PnpSysprep | PersistAllDeviceInstalls set to true. 

### Run Sysprep
From command line:
```
C:\Windows\System32\Sysprep\sysprep.exe /generalize /shutdown
```

### How to reset Windows to Out of Box Experience(OOBE)?
https://askleo.com/set-up-windows-10-again-with-the-windows-out-of-box-experience-oobe/
https://docs.microsoft.com/en-us/windows-hardware/customize/desktop/customize-oobe
https://docs.microsoft.com/en-us/windows-hardware/customize/desktop/customize-oobe-in-windows-11

## Useful Links
* [Sysprep (Generalize) a Windows installation](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation?view=windows-11)