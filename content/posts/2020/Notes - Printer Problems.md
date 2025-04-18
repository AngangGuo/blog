---
title: "Notes - Printer Problems"
date: 2020-07-06T15:27:41-07:00
categories:
 - Tech
 - Printer
tags:
 - Datamax
 - Zebra
 - GT420t
 - GT420d
draft: false
---

## Models
### What's the difference between GK420d and GK420t?
GK420d = direct thermal (no printer ribbon facility), 
requires labels to be made from a direct thermal material in order to create print.

GK420t = thermal transfer (option of a printer ribbon), 
takes labels made from normal stocks but they can also print onto direct thermal materials too.

GK420t is more expensive than GK420d. If you use direct thermal only, GK420d is the best choice. 

### What about Zebra ZD401?
ZD401 can print 2in x 1in label. ZD401 can print up to 2.25in label, but GK420d can print up to 4.25in labels. 

Zebra contact information:
The Inquiry Department can be contacted at 877-208-7756 or at [contact.us](http://contact.us) @zebra.com.

## Troubleshooting

### GK420t keeps skipping labels
One of our GK420t printer keeps skipping labels.

![GK420t Skip Label](/images/2020/gk420t-skip-labels.PNG)

The cause of a Zebra printer skipping labels is usually due to the printer not being fully calibrated or 
that your driver settings are not set properly for the label size that you are using. 

In our case, all the printer settings look good. The printer need to be calibrated.
Calibrating ensures the correct and consistent positioning of the label in the printer, 
and the print on the label.

Follow these steps to fix the problem:

* Load labels
* Power on the printer.
* Press and hold the `Feed` Button until it flashes two times, and then release it
* Allow your printer to complete feeding labels

The printer should now be calibrated. To test – press the feed key and one label should be issued each time.

### GK420t Printer - Labels stuck around the roll
To remove the labels on the Platen Roller and clean it, 
pull the two tabs out and pull it out.

![GK420t Platen Roller](/images/2020/GK420t-Roller.PNG)

Or watch this video: https://www.youtube.com/watch?v=BEJtSEb_Otg

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

