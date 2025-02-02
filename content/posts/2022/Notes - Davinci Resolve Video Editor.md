---
title: "Notes - Davinci Resolve Video Editor"
date: 2022-06-06T16:00:13-07:00
categories:
  - Church
  - Media
tags:
  - Video Editor
  - Davinci Resolve
draft: false
---

## Tips
### Shortcuts
* Ctrl+Shift+,: Move clip to left
* ctrl+Shift+.: Move clip to right
* Ctrl+Shift+v: Paste insert

## FAQ
### How to grab a still picture from current timeline?
File menu > Export > Current Frame as Still. 
You can set a shortcut via Keyboard Customization.
You can pick file type in the dropdown, inc. png, dpx or tiff for uncompressed. 
Resolution is timeline's.

## Photo Slide Show
### Tips
* Make sure to pre-resize your photo to a similar size, otherwise some photos may very large and will eat up all your system resources when rendering

### Photo Slide Show Technics
* Set the default photo length:
`Preferences > User > Editing > General Settings > Standard Still Duration`
* Zoom the photo: 
`Inspector > Video > Transform > Zoom > xxx`
* Add a border to a photoA: 
  1. Right-click the photo in timeline, choose `Open in Fusion Page`
  2. `Effects > Templates > Edit > Effects > Colored Border`
  3. Drop the `Colored Border` effect at the middle of the MediaIn1 and MediaOut1
* Add similar border to other photos: Select all the other photos, then click the photoA with the middle button to copy the border effect
* Fill the background with blurred picture: `Effects > Open FX > Resolve FX Stylize > Blanking Fill`, you can change the settings from Inspector tab
