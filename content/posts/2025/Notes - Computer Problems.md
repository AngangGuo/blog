---
title: "Notes - Computer Problems"
date: 2025-01-24T10:33:26-08:00
categories:
  - Tech
  - Work
tags:
  - Repair
  - PC
draft: false
---

### PC won't trun on
1. When press the power button, nothing happens
2. Touch the power pins(6,8) with a metal screwdriver it works well and can turn the computer off using the power button. 
3. Checked the power cable from front panel and found that it connected to the wrong pins. 
Reconnect it to the power pins and the power switch works.

![motherboard power pins](/images/2025/motherboard-power-pins.png)

### Black Screen
When turned the HP laptop on, you can hear the fans, HDDs are working but the screen is black.

Step 1:
* Turn power off and remove unplug the power cable
* Remove power cable and battery from laptop
* Hold power key about 15 seconds
* Connect the power cable to see if there's any display

Step 2:
If step 1 fails, connect a monitor to the laptop to see if the display adapter is OK.
If the other monitor works, update the display driver and the laptop monitor may works.

Step 3:
Reinstall memory card and other removable parts. (clean the connector)

Step 4:
For HP laptop:
Win + B + Press Power button, when computer turned on, release all the keys, HP laptop will restore BIOS.