---
title: "Quick Video Editing Using FFmpeg"
date: 2020-04-04T16:56:37-07:00
categories:
 - Tech
 - Media
tags:
 - FFmpeg
 - Video
draft: false
---

[FFmpeg][1] is a free and open-source software. It's a very fast video and audio converter that can also grab from a live 
audio/video source. It can also convert between arbitrary sample rates and resize video on the fly with 
a high quality polyphase filter. 

I got some video and audio files need to be processed. 

## Tips
### How to change video/audio volume?
```
// video
ffmpeg -i input.mp4 -af "volume=0.5" -c:v copy output.mp4

// audio
ffmpeg -i input.mp3 -af "volume=0.5" output.mp3
```
`volume=0.5`: half the volume; 
a value less than 1 decreases the volume, and a value greater than 1 increases it.

You can also use decibels like `volume=3dB`. 

### Detecting the volume
With `volumedetect`, we can find out how much a track's volume can be increased:
```
ffmpeg -i input.mp3 -af "volumedetect" -vn -sn -dn -f null -
```
See [link](https://creatomate.com/blog/how-to-change-the-volume-of-a-media-file-using-ffmpeg)

### How to cut video from A to B?
You may want to remove some part of the video and keep another part. 
Maybe there're copyright music in it you need to remove in order to upload it to Youtube, 
or some un-related talking you don't want to share with others.

You can play the video using any video player like [VLC media player][2], 
record the start and end time of the part you want to share.

Suppose the filename is `"My Awesome Video.mp4"`, and you want to keep the video between 5:20 and 1:30:00.
Then using the following command to cut it:
```
ffmpeg -ss 00:05:20 -i "My Awesome Video.mp4 -to 01:30:00 -c copy -copyts -avoid_negative_ts 1 "Keep_cut.mp4"
```

Compare these two commands:
```
// fast but not precise
ffmpeg -ss 00:01:00 -i video.mp4 -to 00:02:00 -c copy cut.mp4

// slow but accurate
ffmpeg -i video.mp4 -ss 00:01:00 -to 00:02:00 -c copy cut.mp4
```
* `-ss` start timestamp
* `-i` input filename
* `-to` end timestamp
* `-c copy` keep original video and audio format
* `-copyts` keep original video file timestamp. The `-to` timestamp will be the original timestamp 

**cut.bat**
```
@echo off
echo -------------------------------------------------------------------------
echo.
echo Usage: acut audiofile.mp4 start-time end-time
echo.
echo Example: acut 20200401.mp4 00:00:30 00:10:30
echo.
echo -------------------------------------------------------------------------
echo. 

echo // long timestamp keeps at the beginning of the new video looks strange
echo C:\Tools\ffmpeg7\ffmpeg -ss %2 -i %1 -to %3 -c copy -copyts -avoid_negative_ts 1 "%~n1_cut%~x1"

echo // end time not correct
echo C:\Tools\ffmpeg7\ffmpeg -ss %2 -i %1 -to %3 -c copy "%~n1_cut%~x1" 

echo // black beginning
echo C:\Tools\ffmpeg7\ffmpeg -i %1 -ss %2 -to %3 -c copy -copyts -avoid_negative_ts 1 "%~n1_cut%~x1"

echo // this command works well
C:\Tools\ffmpeg7\ffmpeg -ss %2 -to %3 -i %1 -c copy -map 0 -avoid_negative_ts make_zero "%~n1_cut%~x1"
```

Note:
The above command is for .mp4 files, for .mov file, see [here](https://unix.stackexchange.com/questions/778153/trimming-a-video-using-ffmpeg-leads-to-black-screen)

### How to convert a video to 720p?
Use the following commands to convert a video to 720P or 360P. See [Scaling][8]
```
// 720P
// If we'd like to keep the aspect ratio, we need to specify only one component, 
// either width or height, and set the other component to -1.
ffmpeg -i Lesson9.mp4 -vf scale=-1:720 -c:v libx264 -crf 23 -preset fast -c:a copy Lesson9-720P.mp4
```

Problem: The video image can't show out when I reply it using VLC. 
Use the following command fix the problem.
```
// 360P
// Some codecs require the size of width and height to be a multiple of n. 
// You can achieve this by setting the width or height to -n:
// This is my case:
ffmpeg -i Lesson9.mp4 -vf scale=720:-2 -preset veryfast -c:a copy Lesson9-360P.mp4
```

### How to convert audio file to video file?
You can add a picture to the audio file and convert it as a video file.

Suppose you have `Awesome Speech.m4a` and `Beautiful View.jpg`, you can use the following command to
convert them to a video file:
```
ffmpeg -loop 1 -i "Awesome Speech.m4a" -i "Beautiful View.jpg" -c:v libx264 -vf format=yuv420p -c:a copy -shortest "Wonderful Video.mp4"
```
* `-c:v libx264` H.264 Encoding(mp4)
* `-vf format=yuv420p` pixel format - for compatibility; see note
* `-shortest` Use -shortest to tell it to stop after the audio stream is finished.

**Note:**
* When outputting H.264, adding `-vf format=yuv420p` or `-pix_fmt yuv420p` will ensure compatibility so crappy players can decode the video.
See [Slideshow][9]
* For GIFs, the option is `-ignore_loop 0`

You can use the following batch file to convert:

**atov.bat**
```
@echo off
echo -------------------------------------------------------------------------------
echo.
echo Usage: atov imagefile.mpg audiofile.m4a
echo.
echo Example: atov dawn.jpg 20200401.m4a
echo.
echo -------------------------------------------------------------------------------
echo. 
ffmpeg -loop 1 -i %1 -i %2 -c:v libx264 -vf format=yuv420p -c:a copy -shortest "%~n2.mp4"
```

### How to merge multiple video files?
* Create a video file list in the order you want to merge. Example:
```
file video1.mp4
file video2.mp4
...
```

* Use the following command to merge it
```
ffmpeg -f concat -safe 0 -i mylist.txt -c copy all.mp4
```

* The following batch file can merge all the mp4 video files in the folder if the file order is not important.

**mergeall.bat**
```
REM Copy all the mp4 part files into a folder
REM CD to this folder and run this command
@echo off
(for %%i in (*.mp4) do @echo file '%%i') > mylist.txt
ffmpeg -f concat -safe 0 -i mylist.txt -c copy all.mp4
del mylist.txt
```
See [Concatenate][6]

### How to extract audio from video file?
```
// simplest
ffmpeg -i video.mp4 audio.mp3

// custom bitrate
ffmpeg -i video.mp4 -b:a 192k -vn audio.mp3
```

Note: You can also use [Audacity](https://www.audacityteam.org/) to convert video file to mp3

### How to convert audio format?
```
ffmpeg -i "20191201.mp3" -c:a aac a.m4a
```
M4A file is compressed while with lossless quality, which means you get smaller(than mp3) file with original quality.

### How to convert WMA to MP3?
```
ffmpeg.exe -i sample.wma -codec:a libmp3lame -ab 128000 -id3v2_version 3 -write_id3v1 1 sample.mp3
```

For bulk converting WMA to MP3 files:
```
FOR /F "tokens=*" %G IN ('dir /b *.wma') DO ffmpeg -i "%G" -codec:a libmp3lame -ab 128000 -id3v2_version 3 -write_id3v1 1 "%~nG.mp3"
```

You can use a simple tool [File Converter](https://file-converter.org/) to convert audio files.

### How to extract a picture from a video file?
See [FFmpeg Wiki][7]
```
ffmpeg -i input.flv -ss 00:00:14.435 -vframes 1 out.png
```

Or see [here](https://stackoverflow.com/questions/27568254/how-to-extract-1-screenshot-for-a-video-with-ffmpeg-at-a-given-time)
on how to control output quality by using `-q:v` option.
```
ffmpeg -ss 01:23:45 -i input -frames:v 1 -q:v 2 output.jpg
```

### How to convert a picture to video file?
```
ffmepg -framerate 30 -loop 1 -i Slide1.jpg -c:v libx264 -t 5 out.mp4
```
* -framerate: should be the same as your video frame rate
* -t: video length (seconds)
* -vf scale=1920:1080: if you need to scale the picture

###  Convert video file
```
ffmpeg -i in.mp4 -s 320x180 -b:v 1500k -b:a 128k out.mp4
```

### Convert 608X1080 to 1080X720 and padding with blue
```
ffmpeg -i in.mp4 -vf “scale=iw*720/1080:720,pad=1080:720:0:0:blue” out.mp4 
```

**Tips**
If you want to use batch file to convert/process the video files, put all the batch files and `ffmpeg.exe` in the same folder,
and including the following batch file. You can double click this file to open the command line window:

**Start.bat**
```
@echo off
set PATH=%~p0;%PATH%
cd /d %~dp0
cmd.exe
```

[1]: https://www.ffmpeg.org/
[2]: https://www.videolan.org/vlc/index.html

[6]: https://trac.ffmpeg.org/wiki/Concatenate
[7]: https://trac.ffmpeg.org/wiki/Create%20a%20thumbnail%20image%20every%20X%20seconds%20of%20the%20video
[8]: https://trac.ffmpeg.org/wiki/Scaling
[9]: https://trac.ffmpeg.org/wiki/Slideshow