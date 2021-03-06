# Fedora-35-Post-Install
Basic setup guide for Fedora 35 after installation

   ### To sync hardware clock with local time in case of a dual boot setup with Windows
   >`sudo hwclock --systohc --localtime`
   
   ### Make use of the fastest mirror for updates
   >`echo 'fastestmirror=1' | sudo tee -a /etc/dnf/dnf.conf`
   >
   >`echo 'max_parallel_downloads=10' | sudo tee -a /etc/dnf/dnf.conf`
   >
   >`echo 'deltarpm=true' | sudo tee -a /etc/dnf/dnf.conf`
    
   ### Add repos providing free and nonfree/proprietary softwares like nvidia drivers, pycharm etc
   >`sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm`
   
   ### Contains many dependencies for other packages. Should be available in system by default.
   >`sudo dnf install dnf-plugins-core -y`
   
   ### Update the system before proceeding further
   >`sudo dnf update -y`
   >
   >`sudo systemctl reboot`
   
   ### Installs the latest nvidia driver (Requires the latest kernel)
   >`sudo dnf install akmod-nvidia`
   
   ### checks the installed nvida version
   >`modinfo -F version nvidia`
   
   ### To support ffmpeg compiled with NVENC/NVDEC
   >`sudo dnf install -y xorg-x11-drv-nvidia-cuda-libs`
   
   ### For hardware video acceleration
   >`sudo dnf install -y vdpauinfo libva-vdpau-driver libva-utils`
   
   ### Installs support for Vulkan which has the same functionality as openGL
   >`sudo dnf install -y vulkan vulkan-tools`
   
   ### To resolve issues that comes with nvidia when suspending system (Install only if suspend doesn't works)
   >`sudo dnf install xorg-x11-drv-nvidia-power`
   >
   >`sudo systemctl enable nvidia-{suspend,resume,hibernate}`
   
   ### Useful for querying repositories
   >`sudo dnf install dnf-utils -y`
   
   Example: `sudo repoquery -i discord`
   
   ### Provides Appstream metadata to enable users to install packages using Gnome Software/KDE Discover
   >`sudo dnf groupupdate core`
   
   ### Adds repos which may contain packages illegal in some countries
   >`sudo dnf install rpmfusion-free-release-tainted`
   >
   >`sudo dnf install rpmfusion-nonfree-release-tainted`
   
   ### Adds additional multimedia codecs
   >`sudo dnf groupupdate multimedia --setop="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin`
   >
   >`sudo dnf groupupdate sound-and-video`
   
   ### To play DVD
   >`sudo dnf install -y libdvdcss`
   
   ### (Optional - helpuful for manual firmware upgrades)
   >`sudo dnf install \*-firmware`
   
   ### For gnome-tweak-tool and gnome-firmware software 
   >`sudo dnf install gnome-tweak-tool gnome-firmware -y`
   
   ### Some more multimedia codecs
   >`sudo dnf install -y gstreamer1-plugins-{bad-*,good-*,ugly-*,base} gstreamer1-libav --exclude=gstreamer1-plugins-bad-free-devel ffmpeg gstreamer-ffmpeg`
   >
   >`sudo dnf install -y lame* --exclude=lame-devel`
   >
   >`sudo dnf group upgrade --with-optional Multimedia`
   >
   >`sudo dnf install -y gstreamer1-plugin-openh264 mozilla-openh264`
   >
   >`sudo dnf config-manager --set-enabled fedora-cisco-openh264`
   >
   >`sudo dnf install -y gstreamer1-plugin-openh264 mozilla-openh264`
   
   ### Installs vscode (check vscodium for the non-telemetry open source version of vscode)
   >`sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc`
   >
   >`sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'`
   >
   >`dnf check-update`
   >
   >`sudo dnf install code`

   ### Stopping NetworkManager and wpa_supplicant periodic scanning
   > Open Settings -> Wifi -> Open your connection's settings -> Identity tab -> Open BSSID dropdown menu -> select the topmost option -> apply -> reconnect
   
   **Reason**: NetworkManager makes wpa_supplicant scan for networks/AP periodically. Only way to stop this is to assign a BSSID to the connected network. By doing this NetworkManager understands that I do not want to roam and will disable the periodic scanning behavior.
   
## Issue Fixes (Do not apply below fixes without facing the issues first)
   ### Desktop glitching on resume Nvidia 495.44
   >change NVreg_PreserveVideoMemoryAllocations from 1 to 0 in /usr/lib/modprobe.d/nvidia-power-management.conf

   ### Gnome-Software won't show flatpak apps
   >Open System Monitor, end process of Gnome-Software and then start Gnome-Software again

   ### Slow Internet Issue
   >change /etc/systemd/resolved.conf to the following
   ```
   [Resolve]
# Some examples of DNS servers which may be used for DNS= and FallbackDNS=:
# Cloudflare: 1.1.1.1#cloudflare-dns.com 1.0.0.1#cloudflare-dns.com 2606:4700:4700::1111#cloudflare-dns.com 2606:4700:4700::1001#cloudflare-dns.com
# Google:     8.8.8.8#dns.google 8.8.4.4#dns.google 2001:4860:4860::8888#dns.google 2001:4860:4860::8844#dns.google
# Quad9:      9.9.9.9#dns.quad9.net 149.112.112.112#dns.quad9.net 2620:fe::fe#dns.quad9.net 2620:fe::9#dns.quad9.net
DNS=1.1.1.1#cloudflare-dns.com 1.0.0.1#cloudflare-dns.com 2606:4700:4700::1111#cloudflare-dns.com 2606:4700:4700::1001#cloudflare-dns.com
FallbackDNS=8.8.8.8#dns.google 8.8.4.4#dns.google 2001:4860:4860::8888#dns.google 2001:4860:4860::8844#dns.google
#Domains=
DNSSEC=no
#DNSOverTLS=no
#MulticastDNS=no
#LLMNR=resolve
#Cache=yes
#CacheFromLocalhost=no
#DNSStubListener=yes
#DNSStubListenerExtra=
#ReadEtcHosts=yes
#ResolveUnicastSingleLabel=no
```
   >`sudo systemctl restart systemd-resolved`

