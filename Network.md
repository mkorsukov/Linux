# LAN setup (with PfSense)

## Setup DNS over HTTPS and TLS

```sh
nmcli dev show

sudo nmcli connection modify netplan-enp3s0 ipv4.dns "10.10.10.10"
sudo nmcli connection modify netplan-enp3s0 ipv4.ignore-auto-dns yes
sudo nmcli connection down netplan-enp3s0
sudo nmcli connection up netplan-enp3s0

nmcli dev show enp3s0 | grep DNS
resolvectl status
```