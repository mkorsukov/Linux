# Ubuntu


## Set primary DNS

```sh
sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
```

## Set monday as first day of week

```sh
sudo nano /etc/default/locale
sudo nano /usr/share/i18n/locales/en_US
```

Add new line with `first_weekday 2`.

```sh
sudo locale-gen
```

## Add archiver

```sh
sudo apt install file-roller
```

## Clean log files

```sh
clear && find $HOME -name "*.log" -type f -exec rm {} \;
```

## GNOME

```sh
gsettings set org.gnome.mutter center-new-windows true
gsettings set org.gnome.desktop.background show-desktop-icons false
gsettings set org.gnome.shell.extensions.dash-to-dock click-action 'minimize'
gsettings set org.gnome.desktop.screensaver restart-enabled true
```

## .NET 10 (manual install)

```sh
curl -L https://dot.net/v1/dotnet-install.sh -o dotnet-install.sh
./dotnet-install.sh --version latest --install-dir /usr/lib/dotnet
```

## Disable access timestamps

```sh
sudo nano /etc/fstab
```

Example of using `noatime` and `nodiratime`.

```
/dev/nvme0n1p2 / ext4 defaults,noatime,nodiratime 0 1
```

## Temp drives

Create directory `/mnt/ram`.

```sh
sudo nano /etc/fstab
```
Add lines:

```
tmpfs /tmp tmpfs rw,nosuid,nodev,noatime,nodiratime,size=4096M,mode=1777 0 0
tmpfs /mnt/ram tmpfs rw,nosuid,nodev,noatime,nodiratime,size=16384M,x-gvfs-hide 0 0
```