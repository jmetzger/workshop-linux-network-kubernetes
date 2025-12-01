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
netplan apply 
```

## Schritt3: dhcp laufen installieren 

```
apt update
apt install kea # Nachfolger von isc-dhcp-server 
# nicht ordentlich gestartet, weil config noch nicht eingerichtet
systemctl status kea 



```

## Schritt 4: config aufbauen 

```
nano /etc/default/isc-dhcp-server
INTERFACESv4="enp0s9"
```


```
nano /etc/dhcp/dhcpd.conf
```

  * mit ip route:  10.0.2.2


```
authoritative;

default-lease-time 660;
max-lease-time 6300;

# range of subnet
range 192.168.0.10 192.168.0.20;

# gateway address
#option routers 192.168.0.1;

# DNS server address
#option domain-name-servers 8.8.8.8, 8.8.4.4;
#}

```

