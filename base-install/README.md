# Base Installation Instructions

1. Boot Arch ISO

2. Connect to wifi using `iwctl`
```bash
# <DEVICE> is your wifi device such as wlan0
# <SSID> is the SSID of the wifi connection
iwctl
[iwd]# station <DEVICE> scan
[iwd]# station <DEVICE> connect <SSID>
```
3. Update system clock with `timedatectl`

4. Partition the disk with a new GPT partition table using `fdisk /dev/nvme0n1`

| Mount Point | Partition | Type | Size |
| --- | --- | --- | --- |
| /boot | /dev/nvme0n1p1 | EFI System Partition (1) | 1G |
| [SWAP] | /dev/nvme0n1p2 | Linux Swap (19) | Same as RAM |
| / | /dev/nvme0n1p3 | Linux x86-64 root (23) | Remainder of Disk |

5. Encrypt system

```bash
# Erase existing LUKS keys and header
cryptsetup erase /dev/nvme0n1p3
wipefs --all /dev/nmve0n1

# Add LUKS
cryptsetup -v luksFormat /dev/nvme0n1p3

# Open LUKS to /dev/mapper/root
cryptsetup open /dev/nvme0n1p3 root
```

6. Format partitions
```bash
mkfs.ext4 /dev/mapper/root
mkswap /dev/nvme0n1p2
mkfs.fat -F 32 /dev/nvme0n1p1
```

7. Mount partitions

```bash
mount /dev/mapper/root /mnt
mount --mkdir /dev/nvme0n1p1 /mnt/boot
swapon /dev/nvme0n1p2
```

8. Configure installation medium's pacman
```bash
# Backup mirrorlist
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.backup

# Add custom mirrorlist
curl https://raw.githubusercontent.com/MSCompSci/arch-configs/refs/heads/main/configs/mirrorlist > /etc/pacman.d/mirrorlist

# Set ParallelDownloads to 10 in /etc/pacman.conf
rnano /etc/pacman.conf
```

9. Install essential packages using `pacstrap -K /mnt <PACKAGES>`
- base
- linux
- linux-firmware
- amd-ucode or intel-ucode
- dracut
- base-devel
- sudo
- e2fsprogs
- dosfstools
- iwd
- nano

10. Generate fstab and edit
```bash
genfstab -U /mnt >> /mnt/etc/fstab

# Change /boot umasks to 0077
rnano /mnt/etc/fstab
```

11. Configure networking for new system
```bash
# Set up systemd-resolved stub mode
ln -sf ../run/systemd/resolve/stub-resolv.conf /mnt/etc/resolv.conf

# Get custom DNS config
curl https://raw.githubusercontent.com/MSCompSci/arch-configs/refs/heads/main/configs/dns_servers.conf > /etc/systemd/resolved.conf.d/

# Get iwd config
curl https://raw.githubusercontent.com/MSCompSci/arch-configs/refs/heads/main/configs/main.conf > /etc/iwd/
```

12. Remount and chroot into new system
```bash
# Unmount partition
umount -R /mnt

# Chroot into new system
arch-chroot /mnt

# Remount to use new umasks in fstab
mount -a
```

13. Set time zone and hwclock
```bash
ln -sf /usr/share/zoneinfo/America/New_York /etc/localtime
hwclock --systohc
```

14. Set locales and hostname
```bash
# Edit /etc/locale.gen and uncomment en_US.UTF-8 UTF-8
rnano /etc/locale.gen

# Generate locales
locale-gen

# Create locale.conf and set LANG=en_US.UTF-8
rnano /etc/locale.conf

# Set hostname in /etc/hostname
rnano /etc/hostname
```

15. Generate UKI and initramfs with dracut
```bash
# Get dracut config
mkdir /etc/dracut.conf.d

curl https://raw.githubusercontent.com/MSCompSci/arch-configs/refs/heads/main/configs/dracut-flags.conf > /etc/dracut.conf.d/

# Get UUID for LUKS (/dev/nvme0n1p3)
blkid

# Add UUID to dracut kernel command line parameters
rnano /etc/dracut.conf.d/dracut-flags.conf

# Generate initramfs and UKI
dracut --regenerate-all
```

16. Add dracut hooks and scripts
```bash
# Get scripts
curl https://raw.githubusercontent.com/MSCompSci/arch-configs/refs/heads/main/scripts/dracut-install.sh > /usr/local/bin/

curl https://raw.githubusercontent.com/MSCompSci/arch-configs/refs/heads/main/scripts/dracut-remove.sh > /usr/local/bin/

# Add execution permissions
chmod +x /usr/local/bin/dracut-install.sh

chmod +x /usr/local/bin/dracut-remove.sh

# Get pacman dracut hooks
mkdir /etc/pacman.d/hooks

curl https://raw.githubusercontent.com/MSCompSci/arch-configs/refs/heads/main/hooks/90-dracut-install.sh > /etc/pacman.d/hooks/

curl https://raw.githubusercontent.com/MSCompSci/arch-configs/refs/heads/main/hooks/60-dracut-remove.sh > /etc/pacman.d/hooks/
```

17. Install systemd-boot
```bash
bootctl install
```

18. Set up users and passwords
```bash
# Set root password
passwd

# Create non-root user with wheel group
useradd -m -G wheel <USERNAME>

# Set non-root user password
passwd <USERNAME>

# Set up sudo
## uncomment "%wheel      ALL=(ALL:ALL) ALL"
## set default visudo editor to rnano with "Defaults editor=/usr/bin/rnano"
EDITOR=rnano visudo

# Limit su login
## uncomment "auth required pam_wheel.so use_uid" in two files
rnano /etc/pam.d/su
rnano /etc/pam.d/su-l

# Set login delay
## Add "auth optional pam_faildelay.so delay=4000000"
rnano /etc/pam.d/system-login
``` 

19. Configure pacman
```bash
# Uncomment ParallelDownloads and Multilib
rnano /etc/pacman.conf
```

20. Exit chroot and reboot
```bash
exit

umount -R /mnt

reboot
```

21. Login as <USERNAME> and check that sudo functions correctly

22. Lock root account with `passwd --lock root`

23. Reconnect to wifi with `iwctl`


