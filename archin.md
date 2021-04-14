Default keyboard layout en_US

### Verify UEFI
```
# ls /sys/firmware/efi/efivars
```
#####  No directory, No UEFI
#
### Check internet connection
```
# ping google.com
```

### Update system clock
```
# timedatectl set-ntp true
```

## Partition the disk
### List disks 
```
# lsblk
```
### Partition Table

Type | Disk Partition | Mount | Flag | Size |
---- | ---------- | ----- | ---- | ---- |	
EFI  | /dev/sdX1  |	/mnt/boot/efi | EFI | 512MB
SWAP | /dev/sdX2  | /mnt/[SWAP] | SWAP | xGB
Root | /dev/sdX3  | /mnt/[root]	| Linux x86_64 | XGB

#
### fdisk

```
# fdisk /dev/sdX
```
```
> fdisk

- g to create a new GPT
<<delete exіsting partition if they exists>>

- n Create new partition

1st 512MB(EFI) 
2nd 5GB(swap)
3rd whatever is left(root)
By defualt all partitions are 'Linux filesystem' change each partition to correspondan.

- t  :change a partition type select partition partition type: 1 for EFI
	-L :to list flags
w : write table to disk and exit

```	

## Mount Partitions
### Format partitions

Format EFI partition 
```
# mkfs.vfat /dev/sdaX  
```
Format root 
```
# mkfs.ext4 /dev/sdaX
````
Swap:
```
# mkswap /dev/sdXX
# swapon /dev/sdXX	
```
### Mounting Partitions
```
mount /root --> mount /dev/sdaX /mnt
mkdir /mnt/boot/efi
mount /boot/efi --> mount /dev/sdaX /mnt/boot/efi
```
Install essential packages
```
# pacstrap /mnt base base-devel linux-lts linux-firmaware vim
```

Configuration
Fstab
```
# genfstab -U /mnt >> /mnt/etc/fstab
```
	THINKPADT450
	- If you have an SSD installed, either add the discard option in the fstab for all partitions,

	/dev/sda1       /boot   vfat    relatime,discard,errors=remount-ro 0 1
	/dev/sda2       /       ext4    relatime,discard,errors=remount-ro 0 2
	/sev/sda3       none    swap    sw,discard                         0 0.

	or use fstrim.

	sudo systemctl enable fstrim.timer
	sudo systemctl start fstrim.timer

Chroot into the system
```
# arch-chroot /mnt
```
Enable Network Manager
>Can do it later, I always forget to do it later. 

```
# pacman -S networkmanager
# systemctl enable NetworkManager
```

Time Zone
```
# ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
```
Generate /etc/adjtime
```
# hwclock --systohc
```

Locale
```
# vim /etc/locale.gen
```
Uncomment the one you want & save
```
# locale-gen
```
```
# vim /etc/locale.conf
LANG=en_US.UTF-8
```

Hostname
```
# vim /etc/hostname

whateverhostnameyouwanttoputinhere
```
```
# vim /etc/hosts
<ip-address>	<hostname.domain.org>	<hostname>
127.0.0.1		localhost
::1				localhost
127.0.1.1 		hostname.localdomain	hostname

```

Generate Root password
```
# passwd 
```

## Bootloader
### EFI
```
# pacman -S grub efibootmgr intel-ucode os-prober
"os-prover if installing alongside MSW10"
# grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
# grub-mkconfig -o /boot/grub/grub.cfg
```

### NOEFI
```
# grub-install --target=i386-pc /dev/sdX
# grub-mkconfig -o /boot/grub/grub.cfg
```
```
#exit
#umount -R/mnt
```
~reboot~

>If everything was done correctly you should go straing into your system, then you can continue.
----------
## POST-INSTALL

 Add users
```
# useradd -m -g wheel,audio,video $user
# passwd $user
```
Add users to sudo
```
# sudo vim /etc/sudoers
```
```
uncomment this stuff:
	## User privilege specification
	##
		root ALL=(ALL) ALL

	## Uncomment to allow members of group wheel to execute any command
 		%wheel ALL=(ALL) ALL
```

## Desktop Environment

Gnome + My usual bullsh

```
# pacman -S xorg xorg-server xorg-xinit gnome noto-fonts zsh zsh-completions man man-pages mesa vulkan-intel mesa-vdpau glu openssh acpi

# sudo systemctl enable gdm.service
```

### PowerManagement
```
sudo pacman -S tlp
sudo systemctl enable tlp.service
```

### My Usual BullSH
```
# pacman -S git htop gthumb code gvim gthumb keepassxc gnome-tweaks neofetch obs-studio audacity 
```

### YAY
```
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```
Installing Oh-My-Zsh
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Copy some conf.
```
git clone https://github.com/rivwoxx/dotfiles.git
```
### Mouse Configuration
```
git clone https://github.com/smasty/g203-led.git
cd g203-led
virtualenv ./env
env/bin/pip install -r requirements.txt
sudo ./g203-led.py solid 34E5EB

```
### Wayland off
#### Wayland is ok but does not work propertly with some apps I use so I use Xorg.
```
Open /etc/gdm/custom.conf and uncomment line

WaylandEnable=false
```
### Grub Screen
```
# vim /etc/default/grub

Change GRUB_TIME=X
```
Generate new grub-config
```
# grub-mkconfig -o /boot/grub/grub.cfg
```
