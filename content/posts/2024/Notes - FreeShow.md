---
title: "Notes - FreeShow"
date: 2024-11-11T06:51:18-08:00
categories:
  - Church
  - Media
tags:
  - FreeShow
  - Presentation
draft: false
---

FreeShow is our free presentation software alternative to ProPresenter.

## Edit
### Keyboard Shotcuts
* Split: Alt+Enter

## Bible

### How to create a bilingual Bible versions slides?
* Select "NIV" and "新标点和合本" from the left panel of the scriptures drawer, right click and press "New collection"
* Click on the collection "EN + CBCS" and select the verses
* Select the Template "Scripture 2" for bilingual scriptures
* Click `New Show` to make the selected bilingual scriptures as a show

![freeshow bilingual bible version](/images/2024/freeshow-bible-collection.JPG)

See also [here](https://github.com/ChurchApps/FreeShow/issues/236#issuecomment-1683423643)

### Add Bible versions
You can find and download Bible versions from [Beblia](https://github.com/Beblia/Holy-Bible-XML-Format).
* [新标点和合本, 上帝版](https://github.com/Beblia/Holy-Bible-XML-Format/blob/master/ChineseCUNPSSBible.xml)

### The book names are in English, how to change them into Chinese?
FreeShow takes the Bible book names from the XML file.
But sometimes they are in English for a different language or missing entirely.

To change the book names, follow the steps below if you already imported it into FreeShow:
* Goto Documents > FreeShow > Bibles,
* Open the saved Bible version.
* Search and replace the book names.
* Run FreeShow and you should see the changes.
```
// original names
[{"name":"Chinese Bible CUNPSS (Simplified) (新标点和合本, 上帝版)","books":[{"number":"1","name":"Genesis",

// You can change the translation name and book names
[{"name":"新标点和合本","books":[{"number":"1","name":"创世纪",
```

If you want to edit the .XML file, you can add a `name=""` attributes to the book tags, like this: `<book name="创世纪"`.
Import it after you added all the book names.

## Output
### How to show different looks to screen and live stream?
I want to show lyrics as full screen to projector and as one third to live stream(NDI) at the same time.

To achieve this, there're several steps:
1. First, create a lower third template(LowerThirdTemplate) for live-streaming from the `Templates` drawer
2. Then, create a new style(LowerThirdStyle) in the `Settings` > `Styles` tab,  
   * `Active layers`: `Background`(Off - Doesn't show video/picture background)
   * `Override slide with template`: LowerThirdTemplate (Show lyrics as lower third)
3. Last, create a new output(LiveStreamOutput) in the `Settings` > `Outputs` tab
   * `Use style`: LowerThirdStyle
   * `Always on top`: Off
   * `Enable NDI`: On (Capture the lower third screen on another computer)
   * `Transparent`: On (Only show the lyrics, backgroud will be transparent)
   * `Invisible window`: On (The LiveStreamOutput window doesn't show out on this computer)
4. Also, enable the NDI of the default output, so that you have option to select one of these two output screens

![freeshow style livestream](/images/2024/freeshow-style-livestream.JPG)

![freeshow output livestream](/images/2024/freeshow-output-livestream.JPG)

### How to make the preview screen bigger?
If there're two or more output windows, each one shows out is smaller.

To make it shows out only one preview window, right-click on the title of other windows and select `Hide from preview`.

See also [Make the output screen bigger](https://github.com/ChurchApps/FreeShow/issues/1059)

### Stage View
https://github.com/ChurchApps/FreeShow/issues/1013
