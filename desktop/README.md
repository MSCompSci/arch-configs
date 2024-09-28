# Desktop setup

1. Install packages
`pacman -S sway alacritty fuzzel xdg-desktop-portal xdg-user-dirs xdg-utils xorg-xwayland qt5ct lxappearance polkit lxqt-policykit noto-fonts flatpak starship ttf-hack-nerd otf-commit-mono`

2. Download and install icons and themes
- [Kora icon theme](https://github.com/bikass/kora)
- [Install icon theme](https://wiki.archlinux.org/title/Icons#Manually)

- [Sweet KDE QT theme](https://store.kde.org/p/1294013)
- [Sweet New Flavor GTK3/4 theme](https://www.gnome-look.org/p/1253385)

- [Oreo Cursors](https://www.gnome-look.org/p/1360254)

2. Set up desktop
```bash
# Create user directors
xdg-user-dirs-update

# Set up starship
# Add "eval "$(starship init bash)" " to the end of ~/.bashrc
starship preset tokyo-night -o ~/.config/starship.toml
source ~/.bashrc

# Set environment variables in /etc/environment
nano /etc/environment
## XDG_CURRENT_DESKTOP=sway
## MOZ_ENABLE_WAYLAND=1
## QT_QPA_PLATFORM=wayland
## QT_QPA_PLATFORMTHEME=qt5ct
## GDK_BACKEND=wayland

# Add sway config
mkdir .config/sway

curl https://raw.githubusercontent.com/MSCompSci/arch-configs/refs/heads/main/configs/config > ~/.config/sway/

```

