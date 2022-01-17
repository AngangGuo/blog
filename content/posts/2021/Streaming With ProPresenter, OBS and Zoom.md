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
To set up ProPresenter send lyrics to OBS: 


## NDI Capture
Capture the whole screen (single or dual monitor)



## OBS Setup
### Installation

Note for `obs-ndi 4.9.0`:
* Only use this release with OBS Studio v25 or above!
* On Windows, you must reboot your computer to make a new or updated NDI Runtime installation effective
* Install [OBS](https://obsproject.com/download)
* Install [obs-ndi plugin](https://github.com/Palakis/obs-ndi/releases)


### Input
OBS can display NDI video and play NDI audio. To show the ProPresenter NDI video:

1. Select a Scene, click `+` to add a source
2. Select `NDI Source` 
3. In the NDI Source properties window, select the ProPresenter NDI output name in the `Source Name` popup
4. (Optional)Add your video source from webcam, video camera or BlackMagic UltraStudio Recorder 3G. Make sure the appropriate mode is selected (like 1080p60, etc) and click Ok.
 
### Output
If Zoom and OBS are in the same computer, you can click `Start Virtual Camera` and select the `OBS Virtual Camera` as video source.

OBS also can act as NDI output source to be used in Zoom from another computer(same computer?). To set NDI output in OBS:

* Select `Tools`, click `NDI Output Settings` 
* Check `Main Output` 
* Main Output Name: (`OBS NDI - Main` or any name)
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

