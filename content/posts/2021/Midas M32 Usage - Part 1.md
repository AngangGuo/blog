---
title: "Midas M32 Usage - Part 1"
date: 2021-10-02T23:13:47-07:00
draft: true
---

## CONFIG/PREAMP
https://en.wikipedia.org/wiki/Microphone_preamplifier

A microphone preamplifier is a sound engineering device that prepares a microphone signal to be processed by other equipment. 
Microphone signals are often too weak to be transmitted to units such as mixing consoles and recording devices with adequate quality. 
Preamplifiers increase a microphone signal to line level (i.e. the level of signal strength required by such devices) by providing stable gain while preventing induced noise that would otherwise distort the signal.[1] For additional discussion of signal level, see Gain stage.

### Gain

### 48V
red light: https://www.youtube.com/watch?v=eTjJvCvO1Qw&t=580s

### Low Cut filter or High Pass filter
Maybe used on everything except Base, Kick, sometimes Tom, Keys.

See https://www.youtube.com/watch?v=2R7qSYa_G8Y
Human can hear 20HZ to 2000HZ
Some high end microphone has built in low cut filter.
When you want to cut the low prequency list air conditional in the room, electric buzz, etc.

Also known as a high pass filter. Basically this is a type of filter that removes low frequencies from an audio signal. 
Normally they are designed so they remove frequencies below a certain determined frequency (often somewhere between 20 Hz and 150 Hz). 
In typical designs these filters have slopes, which means there is more and more attenuation as the frequency gets lower. 
So right around the rolloff (or cutoff) frequency the signal may only be down 3dB to 6dB (3dB is standard), 
but depending upon the design of the filter, lower frequencies may be considerably more attenuated. 
This is usually rated in dB/octave, or decibels per octave of rolloff. 
If your filter is at 150 Hz it is safe to assume the signal will only be reduced by 3dB at that frequency. 
However, one octave below that, at 75 Hz, your signal may be attenuated 15dB, 
or 12dB more. This would represent a 12dB per octave rolloff, which is common.

## Noise Gate
https://en.wikipedia.org/wiki/Noise_gate

https://www.presonus.com/learn/technical-articles/How-To-Use-Dynamics-Processing-Getting-Started-With-Compressors-Gates-and-More

### Compressor VS. Gate
Gates and compressors in its virtual (i.e. plug-in) or hardware form are classified as dynamic processors.

A compressor keeps a signal within a certain dynamic range, determined by the threshold and ratio controls. 
When the signal exceeds the threshold, the volume is lowered by whatever ratio the compressor is set to. 
This is used to smooth out volume levels.

The best way to think of a gate is like an actual gate; it opens and closes to let things (or signal) pass. 
The threshold determines the volume level at which the gate will open. 
The attack control determines how fast the gate will act once the threshold has been reached. 
The release controls how long the gate will stay open. 

A gate won’t pass any signal at all until the signal coming into it reaches the threshold. 
This is used to help eliminate unwanted noise (the hum from an amplifier, leakage from other instruments, etc.) 
by essentially turning off a microphone when all it’s picking up is something other than the source it’s miking.

Some gates can also be adjusted to open and close based on the frequency of the signal. 
These are called frequency dependent gates and are very helpful in keeping hi-hat out of a snare mic and cymbals out of tom mikes.

## Load/Save Scene Settings
### How to save a scene setting?
* Press `View` button in `Show Control` section
* Select the `Scenes` tab in the main display by pressing the right arrow navigation control button
* Select a blank scene and push the save push encoders from the left to save your scene
* Type in the name by using the second right-most `Set` push encoder
* 

### How to load a scene setting?
You can load your scenes following these steps:
* Press `View` button in `Show Control` section
* Select the `Scenes` tab in the main display by pressing the right arrow navigation control button
* Use the left-most load push encoders to select your scene
* Push the load push encoder to load your scene
* Confirm by pushing the right arrow navigation control button

### USB Drive
You can also save/load scenes to/from USB drive.

## Tips
### Select the proper channel
To adjust the settings at the top panel, make sure you select the right channel first.
All the settings adjustment only affect the selected channel.

### Bus Send
* Individual Bus: Select one of the Bus channel, press fider flip button to visualize 
showing the volume of each channel send to this Bus
* Individual Channel: select the channel and set the bus send button on the top panel to send singal to each bus

### Setting take effect
You need to push the EQ/Gate/Low Cut/COMP button at the panel section for the settings to take effect