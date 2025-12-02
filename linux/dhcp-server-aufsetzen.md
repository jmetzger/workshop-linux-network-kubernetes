# DHCP-Server aufsetzen 

## Schritt 1: Ubuntu 22 Runterfahren 

```
... und 3. Netzwerkkarte einrichten
Internal Network 
```

## Schritt 2: Ubuntu 22 wieder hochfahren 

```
# Netzwerk-Interface ausfindig machne
ip a
# -> enp0s9 
```

```
mv /etc/kea/kea-dhcp4.conf /etc/kea/kea-dhcp4.conf.bkup
nano /etc/kea/kea-dhcp4.conf
```

```
# Anlegen _>
# in /etc/netplan/70-config.yaml
# statisch eintragen
# 192.168.0.1
network:
  version: 2
  ethernets:
    enp0s9:
      dhcp4: no
# includes the subnet mask 
      addresses:
        - 192.168.0.1/24
```

```
netplan try
netplan apply 
```

## Schritt3: dhcp laufen installieren 

```
apt update
apt install kea # Nachfolger von isc-dhcp-server 
systemctl status kea-dhcp4-server
```

```
nano /etc/kea/kea-dhcp4.conf 
```

```
# Config anpassen
{
  "Dhcp4": {
    "interfaces-config": {
      "interfaces": [ "enp0s9" ]
    },
    "subnet4": [
      {
        "pools": [ { "pool":  "192.168.0.10 - 192.168.0.20" } ],
        "subnet": "192.168.0.0/24"
      }
    ]
  }
}
```

```
systemctl restart kea-dhcp4-server 
systemctl status kea-dhcp4-server 
journalctl -eu kea-dhcp4-server
```


## 2. Maschine (ubuntu 24.04) hochziehen auch mit internal net

### Schritt 1: runterfahren 

```
poweroff
```

### Schritt 2: 3. Netwerk-Interface Internal Net einrichten 




### Schritt 3: Hochfahren und /etc/netplan einrichten aber mit dhcp 

  * für Interface enp0s9

```
nano /etc/netplan/70-config.yaml
```

```
network:
  version: 2
  ethernets:
    enp0s9:
      dhcp4: true
```

```
chmod 600 70-config.yaml 
netplan try
```

```
# hat ip-adresse bekommen 
ip a
```

```
# DHCP-Maschine anpingen 
ping 192.168.0.1 
```
