# Firewall Regeln setzen 
 
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
firewall-cmd --zone=public --change-interface=enp0s8 --permanent 
```

```
firewall-cmd --direct --add-rule ipv4 filter OUTPUT 0 -m state --state ESTABLISHED,RELATED -j ACCEPT
# Low priority for deny, will get processed as last resort (99)
firewall-cmd --direct --add-rule ipv4 filter OUTPUT 99 -j DROP
```

```
curl -I http://www.google.de
```

```
firewall-cmd --direct --add-rule ipv4 filter OUTPUT 1 -p tcp -m tcp --dport 80 -j ACCEPT
# geht auch ohne -m tcp
firewall-cmd --direct --add-rule ipv4 filter OUTPUT 1 -p tcp --dport 80 -j ACCEPT
```

```
curl -I http://www.google.de 
```

```
firewall-cmd --direct --add-rule ipv4 filter OUTPUT 1 -p tcp -m tcp --dport 53 -j ACCEPT
firewall-cmd --direct --add-rule ipv4 filter OUTPUT 1 -p udp --dport 53 -j ACCEPT
```

```
curl -I http://www.google.de
```


```
# alle Regeln anzeigen lassen
firewall-cmd --list-all
firewall-cmd --get-all-rules --direct 
```

```
# Regeln permanent setzen
firewall-cmd --runtime-to-permanent 
```


```
# Regeln löschen
# Schritt 1: direkte Regeln anzeigen
firewall-cmd --get-all-rules --direct
# Dann Zeile rauskopieren und 
# firewall-cmd --direct --remove-rule davor schreiben
# z.B.
firewall-cmd --direct --remove-rule ipv4 filter OUTPUT 1 -p tcp -m tcp --dport 80 -j ACCEPT
```

## Schritt 4: Logging aktivieren 

```
# geht nur für eingehenden Traffic 
firewall-cmd --get-log-denied
firewall-cmd --set-log-denied=all
journalctl -k | grep -i "REJECT"
```

```
# Logging für ausgehende Regeln vor allgemeiner Deny Regel 
firewall-cmd --direct --add-rule ipv4 filter OUTPUT 98 -j LOG --log-prefix="[DROP]"
journalctl -k | grep "DROP"
```



### Ref:

  * https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/security_guide/configuring_logging_for_denied_packets
