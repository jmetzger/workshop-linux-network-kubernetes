# Firewall egress / ingress 

## Installieren und nur eingehenden Traffic filtern 

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



## Eingehenden und ausgehenden Traffik konfigurieren 


### Step 1: Alow established and drop everything else 


```
firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 0 -m state --state ESTABLISHED,RELATED -j ACCEPT
# Low priority for deny, will get processed as last resort (99)
firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 2 -j DROP
```


```
# firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 1 -p tcp -m tcp --dport 80 -j ACCEPT
Allow HTTPS:

# firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 1 -p tcp -m tcp --dport 443 -j ACCEPT
Allow for DNS queries:

# firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 1 -p tcp -m tcp --dport 53 -j ACCEPT
# firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 1 -p udp --dport 53 -j ACCEPT
Deny everything else:

# firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 2 -j DROP
```




## 🧩 1. Komplettes Skript (Copy & Paste)

```bash
#!/usr/bin/env bash
set -e

# 👇 Passe das Interface an!
IFACE="eth0"

echo "Using interface: $IFACE"

echo "==> Interface in drop-Zone verschieben und drop als Default-Zone setzen ..."
sudo firewall-cmd --permanent --zone=drop --change-interface="$IFACE"
sudo firewall-cmd --set-default-zone=drop

echo "==> Inbound: SSH erlauben (sonst kannst du dich evtl. aussperren) ..."
sudo firewall-cmd --permanent --zone=drop --add-service=ssh

echo "==> Outbound: DNS (53/udp + 53/tcp) erlauben ..."
sudo firewall-cmd --permanent --zone=drop \
  --add-rich-rule="rule family=\"ipv4\" out-interface=\"$IFACE\" destination port port=\"53\" protocol=\"udp\" accept"

sudo firewall-cmd --permanent --zone=drop \
  --add-rich-rule="rule family=\"ipv4\" out-interface=\"$IFACE\" destination port port=\"53\" protocol=\"tcp\" accept"

echo "==> Outbound: HTTP (80/tcp) erlauben ..."
sudo firewall-cmd --permanent --zone=drop \
  --add-rich-rule="rule family=\"ipv4\" out-interface=\"$IFACE\" destination port port=\"80\" protocol=\"tcp\" accept"

echo "==> Outbound: HTTPS (443/tcp) erlauben ..."
sudo firewall-cmd --permanent --zone=drop \
  --add-rich-rule="rule family=\"ipv4\" out-interface=\"$IFACE\" destination port port=\"443\" protocol=\"tcp\" accept"

echo "==> Logging für geblockte Outbound-Verbindungen einrichten ..."
sudo firewall-cmd --permanent --zone=drop \
  --add-rich-rule="rule family=\"ipv4\" out-interface=\"$IFACE\" log prefix=\"FW-DROP-OUT \" level=\"info\" limit value=\"3/m\" drop"

echo "==> Firewall neu laden ..."
sudo firewall-cmd --reload

echo "==> Aktive Zonen:"
firewall-cmd --get-active-zones

echo "==> Konfiguration der drop-Zone:"
firewall-cmd --zone=drop --list-all
```

---

## 🧠 Was dieses Setup macht

* **Zone:** du benutzt jetzt die **`drop`-Zone** als aktive + Default-Zone
* **Default-Policy:** `target=DROP` → alles wird erstmal **gedroppt**
* **Inbound:**

  * nur `ssh` (Port 22) ist erlaubt
* **Outbound (über IFACE):**

  * ✅ DNS (53/udp + 53/tcp)
  * ✅ HTTP (80/tcp)
  * ✅ HTTPS (443/tcp)
  * ❌ alles andere → wird **geloggt & gedroppt**
* **Logging:**

  * via rich rule mit
    `log prefix="FW-DROP-OUT "`
    → findest du später z. B. in `journalctl -xe` oder `journalctl -u firewalld` bzw. im Syslog

---

## 🧪 Tests

Nach dem Skript:

```bash
# DNS-Auflösung testen
dig heise.de @1.1.1.1 || nslookup heise.de

# HTTPS testen
curl -I https://www.google.com

# HTTP testen
curl -I http://example.com

# etwas verbotenen Port testen, z. B. SMTP 25
telnet mail.google.com 25  # sollte NICHT funktionieren

# Logs ansehen
sudo journalctl -xe | grep "FW-DROP-OUT"
```


