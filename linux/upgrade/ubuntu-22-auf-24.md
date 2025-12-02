# Upgrade from Ubuntu 22 auf 24 

## Vorbereitung 

```
# als root 
apt clean all
apt update
apt upgrade
apt dist-upgrade
reboot
apt autoremove --purge
```

### Updater installieren 

```
apt install ubuntu-release-upgrader-core -y
# Sicherstellen, dass wirklich LTS eingestellt ist
```

### Überprüfen, dass wirklich auf die neueste LTS upgegradet wird

```
grep 'lts' /etc/update-manager/release-upgrades
```

### Automatische Installation 

  * Achtung Fremd-Repos können Probleme machen und sollten vorher deaktiviert werden 

```
apt install ubuntu-release-upgrader-core -y
```

  * Danach muss händisch ein Reboot durchgeführt werden

```
sudo reboot
```
