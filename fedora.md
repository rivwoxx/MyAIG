## RPM Fusion

```
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm 
```

## Update

```
sudo dnf -y update
```

- Reboot

## Media Codecs

```
sudo dnf4 group install multimedia
sudo dnf swap 'ffmpeg-free' 'ffmpeg' --allowerasing # Switch to full FFMPEG.
sudo dnf upgrade @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin # Installs gstreamer components. Required if you use Gnome Videos and other dependent applications.
sudo dnf group install -y sound-and-video # Installs useful Sound and Video complementary packages.
```

## VAAPI
```
sudo dnf install ffmpeg-libs libva libva-utils
```

## AMDGPU

```
sudo dnf swap mesa-va-drivers mesa-va-drivers-freeworld
sudo dnf swap mesa-vdpau-drivers mesa-vdpau-drivers-freeworld
sudo dnf swap mesa-va-drivers.i686 mesa-va-drivers-freeworld.i686
sudo dnf swap mesa-vdpau-drivers.i686 mesa-vdpau-drivers-freeworld.i686
```


## OpenH264 for Firefox
```
sudo dnf install -y openh264 gstreamer1-plugin-openh264 mozilla-openh264
sudo dnf config-manager setopt fedora-cisco-openh264.enabled=1
```

* After this enable the OpenH264 Plugin in Firefox's settings.

### My Usual BullSH
```
sudo dnf install steam git htop nvtop fastfetch zsh mpv 
```
## Flatpak SetUp

```
sudo flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

### My Usual BullSH but Flatpak
```
flatpak install flathub com.bitwarden.desktop
```

## CoolerControl

```
sudo dnf install dnf-plugins-core
sudo dnf copr enable codifryed/CoolerControl
sudo dnf install coolercontrol
sudo systemctl enable --now coolercontrold
```

### nct6687 module
```shell
~$ git clone https://github.com/Fred78290/nct6687d
~$ cd nct6687d
```
- Build & install package
```shell
~$ make akmod
```

## ZSH setup
```
chsh -s $(which zsh)
```

### OhMyZSH & Plugins
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

```


## GRUB conf
```
grub2-mkconfig -o /boot/grub2/grub.cfg

Change GRUB_TIME=X

sudo grub2-mkconfig -o /boot/grub2/grub.cfg

```

## Copy some conf.
```
git clone https://github.com/rivwoxx/dotfiles.git
```

## Mouse Configuration
```
git clone https://github.com/smasty/g203-led.git
cd g203-led
virtualenv ./env
env/bin/pip install -r requirements.txt
sudo ./g203-led.py solid 34E5EB

```