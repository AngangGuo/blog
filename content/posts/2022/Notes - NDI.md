---
title: "Notes - NDI"
date: 2022-01-16T21:34:22-08:00
categories:
  - Tech
  - Church
tags:
  - NDI
  - Zoom
  - ProPresenter
draft: false
---

## NDI

### Screen Capture vs Screen Monitor

### How to capture screen? {#capture}
From computer A run `Screen Capture` to share your computer’s desktop video and/or audio to 
any other device on your network with NDI.

* `NDI Launcher` > `Screen Capture` 
* Right click `Screen Capture` icon in Windows taskbar
* `Capture Settings` > `Config Rol` > Resize and move the `NDI Region of Interest` window to your desired location
* Select `Region of interest` to enable screen capture

### How to show Zoom screen to ProPresenter?
* Capture the Zoom Application. See [How to capture screen?]({{< ref "#capture" >}})
* Config the captured Zoom screen as the input of ProPresenter. See [ProPresenter NDI Input]({{< ref "#ndi-input" >}})

### How to change computer name shown in NDI source?
`NDI Access Manager` > Advanced > NDI Device Alias > Set any name you like

## ProPresenter & NDI
### NDI Input {#ndi-input}
We can use NDI signal from other devices at the same network 
as ProPresenter input source and show them out to screen as video.

**NDI Video Input Configuration**
* ProPresenter > Preferences > Inputs
* Click `+` icon to add a new video input
  * `Name`: NDISignalFromPC1
  * `Device`: ASUS6216(VLC)
* Audio
  * `Source`: No Audio Source / Video (Embedded)

**Shown In ProPresenter**
* `Media` > `Video Input` > `+` Add Media > Select input device `NDISignalFromPC1`
* Click the video source to capture video 

Note:
* The video input behavior can be one of these: `Video Input`, `Backgroud` and `Foreground`
* The video input scaling can be one of these: `Scale to Fit`, `Scale to Fill`, and `Stretch to Fill`

### Output
You can also send ProPresenter screen out as NDI signal.

**Screen Configuration**
* `Screens` > `Config Screens...`
* `Audience` > Click `+` to add a new screen 
* `New NDI`
  * Output: asus6212 - NDI 1
  * Name: asus6212 - NDI 1 (You can rename it as anything you like)
  * Mode: 720p59.94 (You can select other mode)
  
From other computers in the same network you'll see this NDI video in NDI Screen Monitor or as another ProPresenter input source.


