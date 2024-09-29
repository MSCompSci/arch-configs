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
- otf-font-awesome
- qtct 
- lxappearance 
- kvantum
- starship 
### WM and Desktop Environment
- sway 
- swaybg
- waybar
- lxqt-policykit 
- alacritty
- fuzzel 
- lximage-qt
- lxqt-archiver
- pcmanfm-qt
- flatpak
- grim
- flameshot
- gvfs
- gedit
- gedit-plugins
- firefox
- chromium
- keepassxc
- code
- gnome-keyring
- seahorse
### Utilities
- git
- github-cli


## 2. Set up desktop
```bash
# Create user directors
xdg-user-dirs-update

# Set default apps
xdg-mime default pcmanfm-qt.desktop inode/directory

xdg-settings set default-web-browser chromium.desktop

# Set environment variables in /etc/environment
nano /etc/environment
```

## 3. Set up keyring
```bash
# Set up keyring
## Add to /etc/pam.d/login
###  auth       optional     pam_gnome_keyring.so
#### session    optional     pam_gnome_keyring.so auto_start
sudo rnano /etc/pam.d/login

## Add to /etc/pam.d/passwd
### password	optional	pam_gnome_keyring.so
sudo rnano /etc/pam.d/passwd

## Set GPG pinentry program to "pinentry-program /usr/bin/pinentry-gnome3"
nano ~/.gnupg/gpg-agent.conf
```

## 4. Set up Git
```bash
# Set git credential helper
git config --global credential.helper /usr/lib/git-core/git-credential-libsecret

# Import key
gpg --import key.asc

# Set username
git config --global user.name "USERNAME"

# Set email
git config --global user.email "EMAIL"
```

## 5. Configure Programs
```bash
# Set up starship
## Add "eval "$(starship init bash)" " to the end of ~/.bashrc
starship preset tokyo-night -o ~/.config/starship.toml
source ~/.bashrc

# Add sway config
mkdir .config/sway

curl https://raw.githubusercontent.com/MSCompSci/arch-configs/refs/heads/main/configs/config > ~/.config/sway/config
```

## 6. Install flatpaks
`flatpak install flathub <PACKAGES>`
- com.github.tchx84.Flatseal
- com.discordapp.Discord

## 7. Logout, login, and restart Sway

## 8. Download and install icons and themes
- [Sweet-Dark theme](https://github.com/EliverLara/Sweet)
- [Sweet Kvantum/QT theme](https://github.com/EliverLara/Sweet/tree/nova)
- [BeautyFolders and BeautyLine Icons](https://store.kde.org/p/1425426/)
- [Layan-Border Cursors](https://store.kde.org/p/1365214/)