# Ubuntu 26.04 - Software Development Setup

## Add RAM disk

```sh
sudo mkdir -p /mnt/ram

sudo nano /etc/fstab
# tmpfs /mnt/ram tmpfs rw,nosuid,nodev,noatime,nodiratime,size=16384M,x-gvfs-hide 0 0
```

## Add software

```sh
# Docker
sudo apt install docker.io docker-compose-v2 -y
sudo usermod -aG docker $USER

# VS Code
wget -O ./vscode.deb https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-x64
sudo apt install ./vscode.deb

curl -L "https://raw.githubusercontent.com/mkorsukov/Dev-Settings/main/visual-studio-code.json" -o ~/.config/Code/User/settings.json

# Azure Data Studio
wget -O ./azuredatastudio.deb https://aka.ms/azuredatastudio-linux-deb
sudo apt install ./azuredatastudio.deb

curl -L "https://raw.githubusercontent.com/mkorsukov/Dev-Settings/main/azure-data-studio.json" -o ~/.config/azuredatastudio/User/settings.json
```

## .NET

```sh
# Auto
sudo apt-get install -y dotnet-sdk-10.0

# Manual
curl -L https://dot.net/v1/dotnet-install.sh -o dotnet-install.sh
./dotnet-install.sh --version latest --install-dir /usr/bin/dotnet
```

## Increase file watch limits

```sh
sudo nano /etc/sysctl.conf
# fs.inotify.max_user_instances=512
# fs.inotify.max_user_watches=524288
```

## Clean log files

```sh
clear && find $HOME -name "*.log" -type f -exec rm {} \;
```