# 🐧 Arch Linux Custom VM Installation – Joaquín Prego

## 🧾 Objective
This project demonstrates the full manual installation of **Arch Linux** on a custom virtual machine, following the Arch Wiki and applying several configurations to create a lightweight and functional desktop system.  
The final VM includes a graphical interface (LXQt), custom shell (ZSH), SSH access, color customization, and user-defined aliases.

---

## 🖥️ Virtual Machine Setup
**VirtualBox Configuration**
- **Base Memory:** 4096 MB  
- **CPUs:** 2  
- **Disk Size:** 20 GB  
- **Chipset:** PIIX3  
- **EFI:** Enabled  
- **Network Adapter:** NAT (Intel PRO/1000 MT Desktop)  
- **ISO Used:** `archlinux-x86_64.iso`

---

## ⚙️ Installation Process

### 1️⃣ Boot Arch ISO
Booted the VM with the official Arch Linux ISO and entered the live environment.

Verified network:
```bash
ping -c 3 archlinux.org
```
Network was working properly inside the live ISO.

---

### 2️⃣ Disk Partitioning
Used `fdisk` to partition the disk `/dev/sda`:
- `/dev/sda1` – EFI System (512MB)
- `/dev/sda2` – Linux filesystem (remaining space)

Formatted partitions:
```bash
mkfs.fat -F32 /dev/sda1
mkfs.ext4 /dev/sda2
```

Mounted partitions:
```bash
mount /dev/sda2 /mnt
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot
```

---

### 3️⃣ Base Installation
Installed base system and kernel:
```bash
pacstrap /mnt base linux linux-firmware vim networkmanager
```

Generated fstab:
```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

Chrooted into new system:
```bash
arch-chroot /mnt
```

---

### 4️⃣ System Configuration
Set timezone:
```bash
ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime
hwclock --systohc
```

Edited locale:
```bash
vim /etc/locale.gen
# Uncommented en_US.UTF-8 UTF-8
locale-gen
```

Hostname and hosts:
```bash
echo "archvm" > /etc/hostname

cat >> /etc/hosts <<EOF
127.0.0.1   localhost
::1         localhost
127.0.1.1   archvm.localdomain archvm
EOF
```

---

### 5️⃣ Users and Sudo
Installed `sudo` and created users:
```bash
pacman -S sudo
useradd -m -G wheel joaquin
passwd 1234

useradd -m -G wheel codi
passwd 1234
```

Enabled sudo for the wheel group:
```bash
EDITOR=vim visudo
# Uncomment: %wheel ALL=(ALL:ALL) ALL
```

---

### 6️⃣ Bootloader
Installed GRUB for EFI:
```bash
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
```

---

### 7️⃣ Enable Network and SSH
Enabled networking and SSH:
```bash
systemctl enable NetworkManager
systemctl enable sshd
```

---

### 8️⃣ Desktop Environment (LXQt)
Installed a lightweight GUI:
```bash
pacman -S xorg lightdm lightdm-gtk-greeter lxqt
```

Enabled LightDM:
```bash
systemctl enable lightdm
```

Rebooted into the graphical desktop successfully 

---

### 9️⃣ Custom Shell (ZSH)
Installed and set ZSH as default shell:
```bash
sudo pacman -S zsh
chsh -s /bin/zsh joaquin
```

Confirmed:
```bash
echo $SHELL
# Output: /usr/bin/zsh
```

---

### 🔟 Custom Aliases and Colors
Edited `.zshrc`:
```bash
nano ~/.zshrc
```

Added:
```bash
autoload -U colors && colors
setopt prompt_subst
PROMPT='%F{green}%n@%m%f %F{blue}%~%f %# '

alias ls='ls --color=auto'
alias ll='ls -lah --color=auto'
alias la='ls -A --color=auto'
alias grep='grep --color=auto'
alias update='sudo pacman -Syu'
alias install='sudo pacman -S'

autoload -Uz vcs_info
precmd() { vcs_info }
zstyle ':vcs_info:git:*' formats '(%b)'
PROMPT='%F{green}%n@%m%f %F{blue}%~%f ${vcs_info_msg_0_}%# '
```

Applied changes:
```bash
source ~/.zshrc
```

Result: colorful, customized prompt visible in blue/green.

---

### 1️⃣1️⃣ AUR Package Installation
Installed **Neofetch** manually from AUR:
```bash
cd /tmp
git clone https://aur.archlinux.org/neofetch.git
cd neofetch
makepkg -si --skippgpcheck
```

Verified installation:
```bash
neofetch
```

Displayed system summary successfully ✅

---

## ✅ Final Configuration Summary

| Feature | Description |
|----------|--------------|
| OS | Arch Linux |
| DE | LXQt |
| Display Manager | LightDM |
| Shell | ZSH |
| Users | joaquin, codi |
| SSH | Enabled |
| Aliases | Configured |
| Colors | Active in prompt |
| AUR Tool | Manual (Neofetch) |
| Boot Mode | EFI |

---

## 🧠 Common Issues Solved
- Network connection issues (fixed by restarting `systemd-networkd` and `systemd-resolved`)
- Missing firmware warnings during `mkinitcpio`
- LightDM conflicts with `sddm`
- GPG keyserver failures (fixed with `--skippgpcheck`)
- yay build failures (bypassed with manual AUR installation)

---

## 🏁 Conclusion
This Arch Linux installation was fully built from scratch, customized, and documented step by step.  
The system now includes all required components and demonstrates understanding of user management, shell customization, desktop configuration, networking, and AUR package handling.
