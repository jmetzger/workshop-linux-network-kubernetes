# Upgrade from Ubuntu 22 auf 24 

## Vorbereitung 

```
# als root 
apt clean all
apt update
apt upgrade -y
apt dist-upgrade
# Optional reboot
# reboot
apt autoremove --purge
```

## Updater installieren 

```
apt install ubuntu-release-upgrader-core -y
```

## Überprüfen, dass wirklich auf die neueste LTS upgegradet wird

```
grep 'lts' /etc/update-manager/release-upgrades
```

## Automatische Installation 

  * Achtung Fremd-Repos können Probleme machen und sollten vorher deaktiviert werden 

```
do-release-upgrade -f DistUpgradeViewNonInteractive
```

  * Danach muss händisch ein Reboot durchgeführt werden

```
# Auf jeden Fall rebooten
sudo reboot
```

## Nach dem Reboot 

```
# ssh war nicht gestartet
systemctl start ssh 
# aktivieren für reboot
systemctl enable ssh
```

```
apt autoremove
```

```
# Laufen alle Services - Probleme zeigen failed 
systemctl list-units -t service
# Optional 
journalctl -b
# Sind alle Interfaces da 
ip a 
```
