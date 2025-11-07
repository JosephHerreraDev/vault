---
date: 2025-02-04
tags:
  - note/tools/archlinux
---
# 1. Identify Your Storage Devices

```bash
lsblk
```

Check the output to identify your disk. For example, if you want to work on `/dev/sdb`:

# 2. Partitioning Using `gdisk`

```bash
gdisk /dev/sdb
```

Inside `gdisk`, the sequence:
- Press **x** to enter expert mode.
- Press **z** to zap (destroy) the `GPT` (`GUID` Partition Table) data structures.
- Press **y** to confirm the deletion of the primary `GPT`.
- Press **y** again to confirm any additional steps if prompted.

# 3. Connect to a Wireless Network

Start the `iwd` (`iwctl`) utility:
```bash
iwctl
```

Then run:
```bash
station wlan0 get-networks
```

Find your network in the list. Replace `network-name` with your actual network name:
```bash
station wlan0 connect network-name
```

# 4. Update the Key-ring and Begin the Installation

```bash
sudo pacman -Sy archlinux-keyring archinstall
archinstall
```

When prompted during installation, choose:

- **Desktop:** `hyprland`  
- **Graphics:** `Nvidia (open kernel module for newer GPUs, Turing+)` drivers
	- `dkms`
	- `libva-nvidia-driver`
	- `nvidia-open-dkms`
	- `xorg-server`
	- `xorg-init`
- **Extra packages**: `man-db`,  `nvim`, `git`, `base-devel`, `ttf-cascadia-code-nerd`, `spotify-launcher` , `rofi`, `starship`, `swaync`, `tmux`, `waybar`, `zathura`, `zathura-pdf-poppler`, `hyprpaper`, `hyprlock`, `yazi`, `lazygit`
# 5. Installing GRUB (EFI Systems)
Assuming an EFI system, install GRUB with:

```bash
grub-install --target=x86_64-efi --efi-directory=/boot --recheck
```

# 6. Installing AUR helper (`yay`)

```bash
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

# 7. Installing Brave

```bash
yay -Sy brave-bin
```

# 8. Wallpaper

```bash
mkdir Pictures
cd Pictures
git clone https://github.com/JosephHerreraDev/wallpapers.git
yay -S waypaper
```

# 9. Spotify + `Spicetify`

```bash
yay -S spicetify-cli
spicetify
spicetify backup apply enable-devtools
yay -S spicetify-themes-git
spicetify config current_theme Sleek 
spicetify config color_scheme Nord
spicetify apply 
```

# 10. Silent Sddm

```bash
yay -S sddm-silent-theme
sudo nvim /etc/sddm.conf
[General]
	InputMethod=qtvirtualkeyboard
	GreeterEnvironment=QML2_IMPORT_PATH=/usr/share/sddm/themes/silent/components/,QT_IM_MODULE=qtvirtualkeyboard

	[Theme]
	Current=silent
```

# 11. Hyprshot

```bash
yay -S hyprshot
```

[[Git ssh configuration]]
[[Gnu Stow for dotfiles]]
