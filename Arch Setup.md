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

- **Desktop:** `hyprland`  
- **Graphics:** `Nvidia proprietary` drivers  
- **Kernel:** `linux-zen`  

You can add additional packages during installation:

```text
Additional packages: 
- git
- pipewire
- lib32-pipewire
- wireplumber
- pipewire-audio
- pipewire-pulse
- pipewire-alsa
- sof-firmware
```

If needed, enable the multilib repository for 32-bit support.

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

# 7. Installing Firefox

```bash
sudo pacman -S firefox
```

---

# 8. SSH Key Setup for GitHub

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
