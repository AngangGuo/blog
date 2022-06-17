---
title: "Notes - UiPath"
date: 2022-06-14T10:36:34-07:00
draft: true
---
## StudioX
StudioX projects are designed for attended use only and we do not recommend using StudioX when developing projects intended for unattended use.

## Troubleshooting

### Microsoft Edge - Can't install extension
See: https://docs.uipath.com/studio/docs/edge-group-policies
https://docs.microsoft.com/en-us/deployedge/microsoft-edge-policies#extensioninstallallowlist

Recommand:
SOFTWARE\Policies\Microsoft\Edge\ExtensionInstallAllowlist\1 = "extension_id1"
SOFTWARE\Policies\Microsoft\Edge\ExtensionInstallAllowlist\2 = "extension_id2"

Actural:
Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Edge\ExtensionInstallAllowlist

Edge: create key: ExtensionInstallAllowlist
1: bhchaenngmlcobfechfkikaofjlmejop 
