## Changing audio sample rate and amplitude

* copy `/usr/share/wireplumber/main.lua.d/50-alsa-config.lua` to `~/.config/wireplumber/main.lua.d/50-alsa-config.lua`
* Uncomment the following and change as necessary
```
["audio.format"]           = "S16LE",
["audio.rate"]             = 44100,
```

**NOTE:** Enable Wireplumber if not running the using the command `systemctl --user --now enable wireplumber`. Check if wireplumber is running using `ps -e| grep wireplumber`.
