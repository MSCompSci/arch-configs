# Desktop setup

## 1. Install packages
`pacman -S <PACKAGES>`
### Basics
- mesa
- pipewire
- wireplumber
- xorg-xwayland
### XDG
- xdg-utils
- xdg-desktop-portal
- xdg-desktop-portal-wlr
- xdg-desktop-portal-gtk
- xdg-user-dirs
### Fonts and Themes
- noto-fonts 
- ttf-hack-nerd 
- otf-commit-mono-nerd
- qtct 
- lxappearance 
- starship 
### WM and Desktop Environment
- sway 
- lxqt-policykit 
- alacritty
- fuzzel 
- flatpak

## 2. Set up desktop
```bash
# Create user directors
xdg-user-dirs-update

# Set up starship
# Add "eval "$(starship init bash)" " to the end of ~/.bashrc
starship preset tokyo-night -o ~/.config/starship.toml
source ~/.bashrc

# Set environment variables in /etc/environment
nano /etc/environment

# Add sway config
mkdir .config/sway

curl https://raw.githubusercontent.com/MSCompSci/arch-configs/refs/heads/main/configs/config > ~/.config/sway/config

```

## 3. Logout and then Login and Start sway

## 4. Install flatpaks
`flatpak install flathub <PACKAGES>`
- org.mozilla.firefox
- com.github.tchx84.Flatseal
- com.discordapp.Discord

## 5. Download and install icons and themes
- [Kora icon theme](https://github.com/bikass/kora)
- [Install icon theme](https://wiki.archlinux.org/title/Icons#Manually)

- [Sweet KDE QT theme](https://store.kde.org/p/1294013)
- [Sweet New Flavor GTK3/4 theme](https://www.gnome-look.org/p/1253385)

- [Oreo Cursors](https://www.gnome-look.org/p/1360254)