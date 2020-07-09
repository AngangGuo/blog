---
title: "Tips - Label Printer"
date: 2020-07-06T15:27:41-07:00
categories:
 - Tech
 - Printer
tags:
 - Datamax
 - Zebra
draft: false
---

## Troubleshooting
### Label Size Problem
**Model:** Datamax - o'neil E-Class Mark III printer(E-4205A)

**Problem:**
The first asset tag can be printer out at the first label. 
All subsequence asset tags can be printed out correctly but they all skip several labels.

It looks as though the setting of the label size is incorrect. 
We changed the printer preference settings to use ScanID(2.00in x 1.00in).
But it keeps skipping labels as though the setting didn't take effect.

![E-4205 Page Preference](/images/2020/E-4205A_Page.jpg)

![E-4205 Page Preference](/images/2020/E-4205A_Graphics.jpg)

![E-4205 Page Preference](/images/2020/E-4205A_Stock.jpg)

 
**Solution:**
You need to change the printer **DEFAULT** settings instead of printer preference settings.

![E-4205 Properties](/images/2020/E-4205A_Properties.JPG)

![E-4205 Advanced Settings](/images/2020/E-4205A_Advanced.JPG)

![E-4205 Default Page Settings](/images/2020/E-4205A_Defaults_Page.JPG)

