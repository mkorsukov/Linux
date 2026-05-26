# Ubuntu 26.04 - Basic Setup

## Disable swap file

```sh
sudo swapoff -a && sudo rm /swap.img
```

## Disable access timestamps

```sh
sudo nano /etc/fstab
# /dev/nvme0n1p2 / ext4 defaults,noatime,nodiratime 0 1
```

## Set log size

```sh
sudo nano /etc/systemd/journald.conf
# SystemMaxUse=200M
sudo systemctl restart systemd-journald
```

## Set SSD trim

```sh
sudo systemctl enable fstrim.timer
```

## Set primary DNS

```sh
sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
```

## GNOME

```sh
gsettings set org.gnome.desktop.background show-desktop-icons false
gsettings set org.gnome.desktop.interface enable-animations false
gsettings set org.gnome.shell.extensions.dash-to-dock click-action 'minimize'
gsettings set org.gnome.mutter center-new-windows true
gsettings set org.gnome.desktop.screensaver restart-enabled true
```

## Remove Snap and packages

```sh
for snap in $(snap list | awk '!/^Name|^core/{print $1}'); do sudo snap remove "$snap"; done
sudo snap remove core24
sudo snap remove snapd
sudo systemctl stop snapd
sudo systemctl disable --now snapd.socket
sudo apt autoremove --purge snapd -y
sudo rm -rf ~/snap /snap /var/snap /var/lib/snapd /var/cache/snapd
sudo apt-mark hold snapd

sudo tee /etc/apt/preferences.d/no-snap.pref << 'EOF'
Package: snapd
Pin: release a=*
Pin-Priority: -10
EOF

sudo tee /etc/apt/preferences.d/no-snapd.pref << 'EOF'
Package: snapd
Pin: release a=*
Pin-Priority: -10
EOF

sudo tee /etc/apt/preferences.d/no-firefox-snap.pref << 'EOF'
Package: firefox
Pin: release a=*
Pin-Priority: -10
EOF
```

## Remove Evolution components

```sh
sudo apt purge evolution evolution-data-server evolution-common evolution-plugins evolution-addressbook -y
sudo apt autoremove -y
rm -rf ~/.local/share/evolution ~/.cache/evolution ~/.config/evolution
sudo apt-mark hold evolution evolution-data-server
```

## Remove unused system packages

```sh
# Speech dispatcher
sudo systemctl stop speech-dispatcher
sudo apt purge speech-dispatcher
sudo apt autoremove

# Update notifier
sudo apt remove --purge update-notifier -y
sudo apt autoremove --purge -y
```

## Disable unused services

```sh
# Online accounts
sudo chmod -x /usr/libexec/goa-daemon

# System daemons
sudo systemctl disable --now fwupd-refresh.timer
sudo systemctl disable --now fwupd-refresh.service
sudo systemctl disable --now fwupd.service
sudo systemctl disable NetworkManager-wait-online.service
sudo systemctl disable --now systemd-binfmt.service
sudo systemctl disable --now apport.service
sudo systemctl disable --now gnome-remote-desktop.service
sudo systemctl disable --now ModemManager.service
sudo systemctl disable --now bluetooth.service
sudo systemctl disable --now switcheroo-control.service
```

## Add missing system packages

```sh
# cURL
sudo apt install curl

# Archiver
sudo apt install file-roller
```

## Add software

```sh
# Media codecs and fonts
sudo apt install ubuntu-restricted-extras -y

# Google Chrome
wget -O ./google-chrome.deb https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install ./google-chrome.deb

# Mozilla Firefox
sudo add-apt-repository ppa:mozillateam/ppa
sudo tee /etc/apt/preferences.d/mozillateam-firefox.pref << 'EOF'
Package: firefox*
Pin: release o=LP-PPA-mozillateam
Pin-Priority: 501
EOF

sudo apt update
sudo apt install firefox-esr -y
```

# https://ubuntuhandbook.org/index.php/2026/04/top-things-to-do-after-installed-ubuntu-26-04-lts