# Configurations to make full use of Wayland in Fedora

## Electron apps
* Enter `gedit ~/.config/electron-flags.conf` and add the following
```
--enable-features=UseOzonePlatform
--ozone-platform=wayland
```
* Enter `ln -s electron-flags.conf ~/.config/electron12-flags.conf`
* Enter `ln -s electron-flags.conf ~/.config/electron13-flags.conf`

## Google Chrome
* `cp /usr/share/applications/google-chrome.desktop ~/Documents/`
* `gedit ~/Documents/google-chrome.desktop`
* Find every line with `Exec=`
* Append `--enable-features=UseOzonePlatform --ozone-platform=wayland` right after `/usr/bin/google-chrome-stable`
* `cp ~/Documents/google-chrome.desktop ~/.local/share/applications/`

## No Audio Issue
* `systemctl --user restart pipewire.service`
* **Note**: run the above without sudo
