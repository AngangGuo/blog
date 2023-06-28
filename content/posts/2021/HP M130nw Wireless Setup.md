---
title: "HP M130nw Wireless Setup"
date: 2021-11-03T21:04:30-07:00
categories:
  - Tech 
tags:
  - Printer
  - Wireless
draft: false
---

I have an HP LaserJet MFP M130nw printer bought years ago. We have several laptops in my home that share this printer.
It's inconvenience to use it by plug/unplug the USB cable every time. 
It will be great if everyone can connect it by using wireless network.

## Setup Printer By Using WPS
We use Telus network. The Telus router supports Wi-Fi Protected Setup (WPS).
Here are how to use (WPS) to connect the printer.

### Method 1: 
* On the printer control panel, press and hold the Wireless button for two or more seconds, 
and then release the button when the wireless light starts blinking.
* Within two minutes, press the WPS button on your wireless router.
* Wait up to two minutes while the printer automatically establishes a network connection with the wireless network.

After the printer connects to the network, the wireless light is on and steady.

### Method 2:
* On the printer control panel, press `Settings` button.
* Use Left/Right arrow key to select `Network Setup` > `OK`.
* Select `Wireless Menu`  > `OK`
* Select `Wi-Fi Protected Setup` > `OK`
* Select `Pushbutton` > `OK`
* Within two minutes, press the `WPS` button on my Telus wireless router.
* Wait up to two minutes while the printer automatically establishes a network connection with the wireless network.

After the printer connects to the network, the screen shows:
```
Connection established.
SSID: TELUS9A06
Channel: 54 (5G)
```

### Setup Laptops
After the printer wireless function works, you can add the wireless printer to each laptop from Windows `Printers & Scanners`.

* Click `Add a printer or scanner` button 
* Select `NPI2796D8 (HP LaserJet MFP M130nw)` wireless printer.
* It will be installed to your laptop and ready to use.

## Setup Wi-Fi Direct Printing
Wi-Fi Direct enables printing from a wireless mobile device without requiring a connection to a network or the Internet.

### Enable Wi-Fi Direct Printing from printer
1. On the printer control panel, press the Setup button.
2. Open the following menus: `Network Setup` > `Wireless Menu` > `Wi-Fi Direct`
3. Choose one of the following connection methods:
   * Automatic: Choosing this option sets the password to 12345678.
   * Manual: Choosing this option generates a secure, randomly generated password.

### Using Wi-Fi Direct Printing from mobile device or Windows computer
1. On the mobile device, open the Wi-Fi or Wi-Fi Direct menu.
2. From the list of available networks, select the printer name.
3. If prompted, enter the Wi-Fi Direct password, or select OK on the printer control panel.

### Install HP Smart App to Windows
The easiest way to use the printer from PC is to [download](https://support.hp.com/us-en/drivers/selfservice/hp-laserjet-pro-mfp-m130-series/9365370/model/9365372) 
and install `HP Smart App`. It'll find the printer automatically. (USB, Newwork or Wi-Fi direct)

## Scan
From `Windows Fax and Scan`, you can also use it as a scanner. 
Also you can use `HP Smart App` to scan document.

## Link
* [HP LaserJet Printers - Wireless Printer Setup](https://support.hp.com/us-en/product/hp-laserjet-pro-mfp-m130-series/9365370/model/9365372/document/c05211196)
* [HP LaserJet MFP M130nw User Guide](http://h10032.www1.hp.com/ctg/Manual/c05208327.pdf)