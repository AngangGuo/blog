---
title: "Live Streaming Using OBS and Zoom"
date: 2020-05-24T22:17:11-07:00
categories:
 - Church
 - Live Streaming
tags:
 - Zoom
 - OBS
draft: false
---

## Startup
1. Open all PowerPoints
    * Worship PowerPoint
    * Message PowerPoint
    * Downloading the songs beforehand if any.
2. Start OBS and VirtualCam
3. Start VoiceMeter
4. Login to Zoom

## Settings
### PowerPoint Settings
NOTE:
* You can't open two PPT at the same time in the old version of PowerPoint. 
Open another PPT as an administrator instead.
* Adjust the PPT window to make sure the presentation window can fit the window size in OBS
* Have a Word doc of all the lyrics as a reference.

#### Worship PPT
Tips:
* The slide must have a certain ratio to be properly integrated into OBS
* Words on the PPT is limited to 2 sets of lines
* It is suggested to have a font size of 50

Settings:
1. Size
    * `Design` tab > `Page Setup` > 
        Slide Size for: Customer;
        Width: 23 inches;
        Height: 2.5 inches;
    * The Width and Height of the slide must be this ratio
2. `Slide Show` > `Slide Show Setup` > `Set Up Show` > `Show Type` >
    * Select `Browse by an individual(window)`
    * Unselect `Show Scrollbar`
3. Press `F5` start the show

#### Message PPT
1. `Slide Show` > `Slide Show Setup` > `Set Up Show` > `Show Type` >
    * Select `Browse by an individual(window)`
    * Unselect `Show Scrollbar` 
2. You may need to adjust the PPT file contents to reserve the spaces for pastor window

### OBS Settings
#### Output Settings
* `Settings` > `Output` > `Streaming` > 
Video Bitrate: 2500 kbps / Encoder: Hardware(NVENC) / Audio Bitrate: 160

#### Audio Settings
* `Settings` > `Audio` > `Devices` > `Desktop Audio` and `Desktop Audio 2`: 
`VoiceMeeter Input(VB-Audio VoiceMeeter VAIO)`

#### Video Settings
* Settings > Video > Base (Canvas) Resolution: 1920 x 1080 / Output: 1920 x 1080 / 
Downscale Filter: Bicubic / FPS: 30

#### VitualCam
Get the instruction on how to install it in OBS from [this link.](https://obsproject.com/forum/resources/obs-virtualcam.949/)
1. Select Tools -> VirtualCam in the main OBS Studio window
2. Press the Start button, then close the dialog
3. Open your program (Zoom, Hangouts, Skype, etc.) and choose `OBS-Camera` as your webcam

#### Re-Link Worship PPT:
* Select scene `Worship/Sub` > `Properties` > `Window`: Select Worship PPT > OK
* Resize in PPT window or OBS if necessary

#### Re-Link Message PPT: 
* Re-link message PPT to scene `Pastor/Slide` 
* 

#### Re-Link Songs
* Select `MP4` scene
* Go to MP4-File sources properties and find the MP4 file
* Uncheck `Loop`
* Select `do nothing when finish`



### VoiceMeter
Hardware Input #1 > Select Input Device > WDM: Microphone (Scarlett 2i4 USB): 

### ZOOM
* Setting > Enable `Original Sound`
* Make sure the participants are not allowed to unmute themselves

#### Audio
* Microphone: VoiceMeeter Output (VB-Audio VoiceMeeter VAIO)
* Speaker: Realtek Digital Output(Realtek High Defination Audio)

#### Video
* Camera: OBS-Camera


## OBS Tips
### How to convert `.mkv` to `.mp4`?
Usually OBS recording is in `.mkv` format. It can't be imported to Davinci Resolve 17 to edit.

You can easily convert the file to `.mp4` format by follow these steps in OBS:

From OBS > `File` > `Remux Recordings`

![OBS Remux Menu](/images/2020/obs-remux.PNG)

Select the `.mkv` file you want to convert, name the target file and click `Remux` to convert it to `.mp4` file.

![OBS Remux File](/images/2020/obs0remux-file.PNG)


