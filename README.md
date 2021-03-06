## Command Line Utilties
These utilities are pretty tailored to me right now and could use librarization and genericizing a bit.

That said, they're quite simple and easy to modify.

### Play Preset
Usage examples:
```
$ play-preset
1. 88.5 XPN - Philadelphia
2. Folk Alley - WXPN - Philadelphia
3. NPR
4. 89.7 WGBH - Boston
5. 89.9 KCRW - Santa Monica
6. 89.9 KCRW Eclectic Music - Santa Monica
7. 88.5 WFCR - Western Mass

$ play-preset 6
Playing 89.9 KCRW Eclectic Music - Santa Monica... OK

$ play-preset folk alley
Playing Folk Alley - WXPN - Philadelphia... OK
```
### Growl 
 - Uses Ubuntu's growl-like notifications to show the current song.
 - Currently has some KCRW-specific tailoring.

`$ soundbridge-growl &`

### UPnP 

Play UPnP audio streams served by Rygel's Gst Launch plugin.

`$ soundbridge-upnp-play`

Playing 'streaming' over the soundbridge

`$ soundbridge-upnp-play speakers`

Playing 'speakers' over the soundbridge


See [Streaming](#Streaming) for how I put it all together.
I have the following pulseaudio and rygel config snippets below.  

```
$ cat ~/.config/rygel.conf # trimmed
[GstLaunch]
enabled=true
launch-items=streaming;speakers
streaming-title=Audio from blip
streaming-mime=audio/x-wav
streaming-launch=pulsesrc device=streaming.monitor ! wavenc
speakers-title=Audio from blip's speakers
speakers-mime=audio/x-wav
speakers-launch=pulsesrc device=alsa_output.pci-0000_00_1b.0.analog-stereo.monitor ! wavenc
```

Setup a streaming sink
```
tail -n 3 /etc/pulse/default.pa
load-module module-null-sink sink_name=streaming format=s16be channels=2 rate=44100 sink_properties="device.description='Streaming'"
```
Stop the music and go home

`$ soundbridge-upnp-stop`

### Streaming

`stream` is very specific to my setup and takes care of all the funny business
of redirecting pulse audio streams around to the various devices I care about.
Namely, the work airtunes speakers, my soundbridge, and my local laptop
speakers.  It's a good example of how to move all that audio around and start
and stop network streaming.  Examples of how I use it:
```
$ stream home
Playing 'streaming' over the soundbridge
Streaming Banshee (18) to home (streaming)

$ stream off
Streaming Banshee (18) to off (0)

$ stream work
Streaming Banshee (18) to work (raop.Conference-Room.local)

$ stream work chrome
Streaming chrome (21) to work (raop.Conference-Room.local)

$ stream off chrome
Streaming chrome (21) to off (0)

$ stream preset npr
Playing NPR... OK
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUyOTg4MTIwNywtNTYxMzE2NTI2LDE4Mz
I2NDMxMyw4MTIwNTU5MzRdfQ==
-->
