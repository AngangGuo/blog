---
title: "Midas M32 Usage"
date: 2021-10-02T23:13:47-07:00
categories:
  - Church
  - Media
tags:
  - Sound System
  - Midas
draft: false
---

## CONFIG/PREAMP
https://en.wikipedia.org/wiki/Microphone_preamplifier

Microphone signals are often too weak to be transmitted to units such as mixing consoles and recording devices with adequate quality. 
Preamplifiers increase a microphone signal to line level by providing stable gain while preventing induced noise 
that would otherwise distort the signal. 

### Gain
On a microphone preamplifier, input gain varies the amount of amplification applied to the microphone.

### Noise Gate
A noise gate is a device that is used to control the **volume** of an audio signal.
Noise gates attenuate signals that register below a certain threshold.

By turning the THRESHOLD rotary control, the audio level at which the gate affects the signal can be controlled

Note: Pressing the GATE button engages the noise gate for the selected channel.


https://en.wikipedia.org/wiki/Noise_gate

https://www.presonus.com/learn/technical-articles/How-To-Use-Dynamics-Processing-Getting-Started-With-Compressors-Gates-and-More

### Compressor VS. Gate
Gates and compressors in its virtual (i.e. plug-in) or hardware form are classified as dynamic processors.

A compressor keeps a signal within a certain dynamic range, determined by the threshold and ratio controls. 
When the signal exceeds the threshold, the volume is lowered by whatever ratio the compressor is set to. 
This is used to smooth out volume levels.

The best way to think of a gate is like an actual gate; it opens and closes to let things (or signal) pass. 
The threshold determines the volume level at which the gate will open. 
The release controls how long the gate will stay open. 

A gate won’t pass any signal at all until the signal coming into it reaches the threshold. 
This is used to help eliminate unwanted noise (the hum from an amplifier, leakage from other instruments, etc.) 
by essentially turning off a microphone when all it’s picking up is something other than the source it’s miking.

Some gates can also be adjusted to open and close based on the frequency of the signal. 
These are called frequency dependent gates and are very helpful in keeping hi-hat out of a snare mic and cymbals out of tom mikes.

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

## DCA
### What's DCA?
DCA is an abbreviation for Digitally Controlled Amplifier.
A DCA is helpful in any situation where you need to adjust multiple channels at the same time.

For example, if you have 10 channels of drums and you wanted to lower their volume without messing up your mix, 
you could go to each channel, one-by-one and lower their volumes by exactly the same amount or 
you could just assign them all to a DCA and use that DCA fader to lower each channel’s volume simultaneously.


### How to assign channels to a DCA?
* Press DCA Group 1-8.
* Press and hold `Select` button of the DCA channel you wish to use.
* Tap the `Select` button for all input channels you want to be assigned to this DCA channel
* Release the `Select` button of the DCA channel to finish the setting.

### What's the difference between DCA and group(Bus)?
A DCA channel is like a remote control, it turns up/down the volume of several channels at the same time.
A Bus group can be 

## Mute Group
### What's a mute group?

### How to set up a mute group?
To assign channels to one of the six mute groups, perform the following steps:
1. Press the `MUTE GRP` screen selection button to switch the main display to the Mute Groups view.
2. Press and hold the desired mute group button on the lower right-hand side of the console's control surface.
3. While holding the mute group button, press the `SEL` button of any input or output channel, on any layer, that you wish to assign to that mute group
4. When you have assigned all of the desired channels to the mute group, release the dedicated `Mute Group` button.

NOTE: The individual channel `MUTE` buttons will remain fully functional during the assignment process, 
only the mute group buttons are blocked.

To use the `MUTE GRP` screen to mute or unmute the groups, perform the following steps:
1. Tap any of the six push encoders to mute the corresponding group, and thus mute all channels that are assigned to that mute group.
2. Tap the push encoder of a currently-muted group to unmute the mute group.
3. When finished working with mute groups, tap the `MUTE GRP` screens election button to exit the `MUTE GRP` screen. 
The entire screen will again display its full set of controls for the current page

## Bus
### How to send Channel 1 & Channel 2 signals to Bus 1?
Method A:
* Select Channel 1
* Press `BUS 1-4` on the BUS SENDS panel
* Rotate the first knob to set the signal send to bus 1
* Repeat the above step for channel 2

Method B:
* Go to Bus 1-8 layer by pressing `Bus 1-8` button
* Select Bus 1
* Press `Fader Flip` button
* Unmute channel 1 and adjust the fader
* Unmute channel 2 and adjust the fader
* Press `Fader Flip` again

Note: (TODO)
Need to confirm:
If all channels are unmuted, you need to adjust the fader to send different signal to Bus
If all channels are muted, then you may only need to unmute the desired channels to send signal to bus.
If you need to send a channel to several bus groups, you don't need to unselect the RL button for that channel
If you send the channel signal to bus and from bus to RL main, then you need to unselect RL for each channel, and select RL for the bus

Channel: remove RL output
Bus: Add RL output


## Matrix Mixes 
See [here](https://www.youtube.com/watch?v=jQ09efrN8MQ) 

A Matrix is a mix of other mixes, or a mix of Subgroups.

#### Channel vs Bus vs Matrix
* You can group several channels into a bus.
* You can group several buses into a matrix.
* You can't send channel signals to matrix

See [here](https://www.youtube.com/watch?v=54UhtUEgw70) for more about matrix

### Why use matrix mixes?
By using Matrix mixes, we have independent mix level control over individual mixes for broadcast truck,
overflow room, cry room, live stream, backstage, etc.
not affecting our FOH mix or any stage mixes.

### How to group buses into matrix?
Press `MTX 1-6/MAIN C` button to go to the Matrix panel > Select the Bus you want to add to the matrix >  
Adjust the matrix fader to indicate the bus signal to send to the matrix. 


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

## Routing
### How to send Bus 1 signal to Output 8?
Routing > out 1-16 > 
* Analog Output: Output 08
* Category: Mix Bus
* Output Signal: MixBus 01
* Tap: Post Fader

## Mis
### How to set Scribble Strip for a channel?
* Press `Setup` > Go to `scribble strip` tab > 
* Select `Channel`, `Color`, `Icon`, 
* Select a name from `Snippets` or type the channel name
* Save

## Tips
### Select the proper channel
To adjust the settings at the top panel, make sure you select the right channel first.
All the settings adjustment only affect the selected channel.

### Setting take effect
You need to push the EQ/Gate/Low Cut/COMP button at the panel section for the settings to take effect