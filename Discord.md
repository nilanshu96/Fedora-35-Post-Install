# Installing Discord

There are two ways to install it in Fedora:

1. Installing the Flatpak using Gnome Store
2. Using the tar.gz from the Discord website

Installing the flatpak is easy. Do the following for installing using tar.gz

1. Download the tar.gz file from Discord website
2. extract and place it in ~ (home) directory
3. create bin folder in ~/.local if it doesn't exists
4. Replace the following
```
Exec=/home/[insert username here]/Discord/Discord --no-sandbox
Icon=/home/[insert username here]/Discord/discord.png
Path=/home/[insert username here]/.local/bin
```
5. Open Discord -> open user settings -> Linux settings -> disable Open Discord and Minimize to Tray

**NOTE:** using `--no-sandbox` can compromise with system security. Be sure of what you're doing before using it.
