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

---

# 2. Partitioning Using `gdisk`

```bash
gdisk /dev/sdb
```

Inside `gdisk`, the sequence:
- Press **x** to enter expert mode.
- Press **z** to zap (destroy) the GPT (GUID Partition Table) data structures.
- Press **y** to confirm the deletion of the primary GPT.
- Press **y** again to confirm any additional steps if prompted.

---

# 3. Connect to a Wireless Network

Start the iwd (iwctl) utility:

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

---

# 4. Update the Keyring and Begin the Installation

```bash
sudo pacman -Sy archlinux-keyring archinstall
archinstall
```

When prompted during installation, choose:

- **Desktop:** `sway`  
- **Graphics:** `Nvidia proprietary` drivers  
- **Kernel:** `linux-zen`
---

# 5. Installing GRUB (EFI Systems)

Assuming an EFI system, install GRUB with:

```bash
grub-install --target=x86_64-efi --efi-directory=/boot --recheck
```

---

# 6. Network Management

For a text-based network configuration tool:

```bash
nmtui
```

---
# 7. Configuration File (sway)

```bash
sudo pacman -S nvim man-db
mkdir -p ~/.config/sway
cp /etc/sway/config ~/.config/sway/
nvim ~/.config/sway/config
```

---
# 8. Configure resolution

```bash
swaymsg -t get_outputs
(in config file)
output <screen-name> resolution <resolution> position 1920,0
```

---
# 9. Installing AUR helper (yay)

```bash
sudo pacman -Sy --needed git base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

---
# 10. Installing nerdfont

```bash
yay -S ttf-meslo-nerd
fc-list | grep Meslo
nvim ~/.config/sway/config
(in config)font pango:Meslo LGM Nerd Font 10
```

---
# 11. Installing Brave

```bash
yay -Sy brave-bin
```

---
# 12. Wallpaper

```bash
mkdir Pictures
cd Pictures
git clone https://github.com/JosephHerreraDev/wallpapers.git
(in configuration file)
output * bg /Pictures/wallpapers/wallpaper.png fill
```

---
# 13. Kitty config

```bash
sudo pacman -S kitty
(in configuration file)
set $term kitty
cp /usr/share/doc/kitty/kitty.conf ~/.config/kitty/kitty.conf
kitten themes
(in kitty.conf)
font_family Meslo LGM Nerd Font
```

---
# 14. Neovim Config

```bash

```

---

# 15 Waybar config

```bash
sudo pacman -S waybar
(in configuration file)
exec waybar
```
---
# 16. Rofi config

```bash
sudo pacman -S rofi
(in config file)
set $menu rofi
bindsym $mod+d exec $menu -show drun
rofi -dump-config > ~/.config/rofi/config.rasi
```

# 17. Spotify + Spicetify

```bash
sudo pacman -S spotify-launcher
yay -S spicetify-cli
spicetify
spicetify backup apply enable-devtools
yay -S spicetify-themes-git
spicetify config current_theme Sleek 
spicetify config color_scheme Nord
spicetify apply 
```

# 18. Silent Sddm

```bash
yay -S sddm-silent-theme
sudo nvim /etc/sddm.conf
[General]
	InputMethod=qtvirtualkeyboard
	GreeterEnvironment=QML2_IMPORT_PATH=/usr/share/sddm/themes/silent/components/,QT_IM_MODULE=qtvirtualkeyboard

	[Theme]
	Current=silent
```

# 15. GNU Stow

```bash

```
---
# 16. SSH Key Setup for GitHub

Add user email and name

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"
```

Generate a new SSH key (RSA 4096-bit):

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

Start the SSH agent:

```bash
eval "$(ssh-agent -s)"
```

Add your key to the agent:

```bash
ssh-add ~/.ssh/id_rsa
```

Display your public key, then copy it to your clipboard:

```bash
cat ~/.ssh/id_rsa.pub
```

Finally, test your SSH connection with GitHub:

```bash
ssh -T git@github.com
```

---
