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
chmod 600 /etc/netplan/70-config.yaml
```

```
netplan try 
```
