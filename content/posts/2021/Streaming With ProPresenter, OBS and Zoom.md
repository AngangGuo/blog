---
title: "Streaming With ProPresenter, OBS and Zoom"
date: 2021-12-17T14:39:21-08:00
categories:
- Church
- Media
tags:
- ProPresenter
- OBS
- Zoom
draft: false
---

## ProPresenter Setup

Warning: This is a work in progress

### How to send contents to OBS in ProPresenter 7?
Screens > Configure Screens...(Ctrl + Alt + 1) > Audience > New NDI > Select the Mode(1080p29.97, etc.)
* Rename the NDI output name(Pro7-NDI) - OBS will see this name as its input source
* Rename the screen name(NDI-Screen) - ProPresenter use this name to identify the NDI screen.

You can customize the NDI screen output by select `Screens` > `Edit Looks...`(Ctrl + !). From the `Looks` screen, 
you can choose which layers(Messages, Props, Announcements, Slide, Media, Video Input), 
and what Presentation theme you want to show in the output. 
See [How To Use ProPresenter in OBS + Lower Thirds](https://www.youtube.com/watch?v=XRxOKMkCcoY) for more details. 

Note: The video used Syphon as example, but the same principles apply to NDI.

**Tips**
* You can mirror the main audience screen and configure the mirror screen as NDI(or Syphon) output send to OBS. 
Or you can create a new NDI screen if you want to have more control(different theme like lower third, etc.).
* NDI screen can't be turned off, it's always on/available. You may need to delete the unused NDI screen to save some local bandwidth.
* In Mac, you can use [Syphon](https://renewedvision.com/blog/beginners-guide-to-syphon/) to send video to OBS.

### How to capture screen to OBS in ProPresenter 6?
In ProPresenter 6, there's no integrated NDI support. 
You can download the [NDI Tools](https://www.ndi.tv/tools/#download-tools)
and run `NDI Screen Capture`(or `NDI Screen Capture HX` if you use NVIDIA GPU-based PC) to send the whole screen as NDI source on the network. 

## OBS Setup
### Installation
Download and install the latest [OBS studio](https://obsproject.com/) and the [obs-ndi plugin](https://github.com/Palakis/obs-ndi/releases).

Note: On Windows, you must reboot your computer to make a new or updated NDI Runtime installation effective

### Input
OBS can display NDI video and play NDI audio. To show the ProPresenter NDI video:

1. Select a Scene, click `+` to add a source
2. Select `NDI Source` 
3. In the NDI Source properties window, select the ProPresenter NDI output name(`Pro7-NDI` for example) in the `Source Name` popup
4. (Optional)Add your video source from webcam, video camera or BlackMagic UltraStudio Recorder 3G. 
5. Make sure the appropriate mode is selected (like 1080p60, etc) and click Ok.
 
### Output
If Zoom and OBS are in the same computer, you can click `Start Virtual Camera` and select the `OBS Virtual Camera` as video source.
OBS also can act as NDI output source to be used in Zoom from another computer(same computer?). 

To set NDI output in OBS:
* Select `Tools`, click `NDI Output Settings` 
* Check `Main Output` 
* Main Output Name: (`OBS-NDI-Main` or any name)
* (Optional) Set the Preview Output

## Zoom
Note: Zoom can't recognize NDI video in my computer yet. Why?
Does it need to be in different computer? need verify.

### How to use video from OBS?
Select the `OBS Virtual Camera`

### How to make OBS NDI work in Zoom?
??

## Troubleshooting
If you are seeing everything sized incorrectly, go to the OBS menu option and click Preferences. 
Then click on the video tab and set the Base (Canvas Size) resolution to whatever your ProPresenter Output is. 


## Useful Links
* [Syncing Video and Audio on Zoom with OBS and NDI](https://www.youtube.com/watch?v=wr9PdkX93WM)
* [How to stream to YouTube](https://obsproject.com/forum/threads/guide-how-to-stream-to-youtube.4333/)

