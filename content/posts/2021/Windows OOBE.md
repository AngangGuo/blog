---
title: "Windows Audit Mode or OOBE"
date: 2021-11-26T14:48:33-08:00
draft: true
---

## Audit Mode
We need to enter Audit mode and install device drivers and pre-install applications.

### How to reset Windows to Out of Box Experience(OOBE)?
Running the OOBE on an already set-up system does not uninstall software or erase any of your data. 
As I said, it’s mostly about configuration and user interface choices.

https://askleo.com/set-up-windows-10-again-with-the-windows-out-of-box-experience-oobe/
https://docs.microsoft.com/en-us/windows-hardware/customize/desktop/customize-oobe
https://docs.microsoft.com/en-us/windows-hardware/customize/desktop/customize-oobe-in-windows-11

### How to keep Administrator account enabled?
See [here](https://support.microsoft.com/en-us/topic/21f02ac5-8fea-f3e9-313a-bb5276e11688)

You go into Sysprep Audit mode from the Out of Box Experience (OOBE) screen.
In Audit mode, the administrator account is enabled immediately before logoff and disabled immediately after logon.
Therefore, the account is locked out when you turn off the computer and then turn it back on.

When the system is using hybrid shutdown (also known as "fast startup") during Audit mode, the account will be disabled.
To work around this behavior, disable hybrid shutdown. To do this, follow these steps:
* Right-click the `Start` button, and then click `Command Prompt (Admin)`.
* At the command prompt, type the following command, and then press Enter:
```
shutdown /s /t 00
```

When you turn on the laptop next time, it'll go directly into Audit mode.

### How to update device driver?
Check the Device Manager, if there are any unknown devices, right-click it and select `Properties`,
update driver, search Windows Update, the driver will be installed automatically from `Windows Update`.

You may need to restart windows by using the above command for the update to take effect.

You may need to repeat the above steps for other unknown devices.

### How to set Windows into OOBE mode?
From System Preparation Tool window
* System Cleanup Action: Enter System Out-Of-Box Experience (OOBE)
* Un-select Generate (Select `Generate` if you want to capture the image and use it from other computers)
* Shutdown Options: Shutdown
* Click `Ok`. Windows will prepare the system into OOBE mode.

### How to run Sysprep by using command line?
From command line:
```
%WINDIR%\system32\sysprep\sysprep.exe 
Or
C:\Windows\System32\Sysprep\sysprep.exe /generalize /shutdown
```

### Prevent Sysprep from removing installed devices
When you set up a Windows PC, Windows Setup configures all detected devices.
Generalizing a Windows installation uninstalls these configured devices, but doesn't remove device drivers from the PC.

If you're deploying an image to computers that have identical hardware and devices as the original PC,
you can keep devices installed on the computer during system generalization by using an unattend file with
Microsoft-Windows-PnpSysprep | PersistAllDeviceInstalls set to true.

## Useful Links
* [Sysprep (Generalize) a Windows installation](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/sysprep--generalize--a-windows-installation?view=windows-11)