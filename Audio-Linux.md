PulseAudio
pulseaudio-equalizer
qpaeq (gui for equalizer)

[pulseaudio equalizer // stackoverflow](https://askubuntu.com/questions/980876/how-do-i-start-pulseaudio-equalizer)
[tutorial](https://linuxhint.com/install-pulseaudio-equalizer-linux-mint/#:~:text=PulseAudio%20Equalizer%20is%20a%20free,from%20an%20external%20PPA%20repository.)
just gotta run the .sh

not quite sure if I want to run that

`aplay -l` - lists all cards
`pacmd list-sinks` - 


### Steps
```
pactl load-module module-equalizer-sink
```

``````
pactl load-module module-dbus-protocol
``````


and, to make these changes permanent, edit `~/.config/pulse/default.pa` (create it if necessary) and add these lines:

```
load-module module-equalizer-sink
load-module module-dbus-protocol
```
finally just run
```qpaeq```


### Unload Modules
```
pactl list short modules
```

```
pactl unload-module 24
```
- unloads the particular module


``````
mv ~/.config/pulse ~/.config/pulse.old
``````
saving old config


*note*
basically all I had to do was get rid of the `~/.config/pulse` files and just start afresh LOL