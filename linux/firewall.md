# Firewall egress / ingress 

## Schritt 1: Installieren und nur eingehenden Traffic filtern 

```
systemctl status firewalld 
apt search ^firewalld 
apt install firewalld -y 
systemctl status firewalld 

firewall-cmd --state 
# Was ist Betrieb für die Zone 
firewall-cmd --list-all 
firewall-cmd --get-active-zones 

# Interfacd zu der Zone hinzufügen 
firewall-cmd --zone=public --add-interface=enp0s8
# jetzt in der public zone 
firewall-cmd --get-active-zones 

firewall-cmd --runtime-to-permanent 

firewall-cmd --list-all 
# Alle Services 
firewall-cmd --get-services 
# 
firewall-cmd --info-service=http 

## service http freischalten 
firewall-cmd --add-service=http 

firewall-cmd --runtime-to-permanent 
```



## Schritt 2: Eingehenden und ausgehenden Traffik konfigurieren 

```
# Alles auf Anfang // nur wenn interface vorher gewechselt wurde 
firewall-cmd --zone=public --change-interface=enp0s8
```

```
firewall-cmd --direct --add-rule ipv4 filter OUTPUT 0 -m state --state ESTABLISHED,RELATED -j ACCEPT
# Low priority for deny, will get processed as last resort (99)
firewall-cmd --direct --add-rule ipv4 filter OUTPUT 99 -j DROP
```

```
firewall-cmd --direct --add-rule ipv4 filter OUTPUT 1 -p tcp -m tcp --dport 80 -j ACCEPT
firewall-cmd --direct --add-rule ipv4 filter OUTPUT 1 -p tcp -m tcp --dport 53 -j ACCEPT
firewall-cmd --direct --add-rule ipv4 filter OUTPUT 1 -p udp --dport 53 -j ACCEPT
```

```
# alle regelnn anzeigen lassen
firewall-cmd --get-all-rules --direct 
```

```
# Regeln permanent setzen
firewall-cmd --runtime-to-permanent 
```
