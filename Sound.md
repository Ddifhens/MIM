
the scarlett first gen usb interfaces are open and as such work perfectly on linux. 
However, for some reason, sometimes although everything seems the same as last boot, an update to the system or just the reboot by itself may mute one or more of the output channels. 
in this case we can use 
```
alsamixer 
```
and navigate the terminal to the desired soundcard, in this case the scarlett interface. 
muted channels will appear as M or, sometimes, MM. 

simply navigating to the appropriate channel and pressing m will unmute it.


### MUSIC PLAYER 

Tauon is the one im using on my machine, it is minimal and pretty. it has to be installed via flatpak and then run on rofi using specifically the drun mode to allow for the specific way in which flatpak packages want to be run. 
in addition the hotkeys to this app can be found here: 
https://github.com/Taiko2k/Tauon/wiki/Keyboard-Shortcuts 