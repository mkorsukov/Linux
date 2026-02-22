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
gsettings set org.gnome.desktop.screensaver restart-enabled true
```

## .NET 10 (manual install)

```sh
curl -L https://dot.net/v1/dotnet-install.sh -o dotnet-install.sh
./dotnet-install.sh --version latest --install-dir /usr/lib/dotnet
```