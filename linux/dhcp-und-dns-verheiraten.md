# Updates an den DNS - Server schicken 

## Stichwort in KEA 

  * DHCP-DDNS Server
  * https://kea.readthedocs.io/en/kea-2.2.0/arm/ddns.html

## Reservierung pro MAC (muss erstellt werden) 

```
"reservations": [{
  "hw-address": "00:11:22:33:44:55",
  "hostname": "myhost"
}]
```

## Walkthrough 

Hier die wesentlichen Schritte für KEA DDNS mit BIND 9:

### 1. TSIG-Key generieren
```bash
tsig-keygen -a hmac-sha256 ddns-key > /etc/bind/ddns.key
```

### 2. BIND 9 konfigurieren (`named.conf`)
```
include "/etc/bind/ddns.key";

zone "example.com" {
    type master;
    file "/var/lib/bind/example.com.zone";
    update-policy {
        grant ddns-key zonesub any;
    };
};
```

### 3. KEA DHCP-DDNS Daemon (`kea-dhcp-ddns.conf`)
```json
{
  "DhcpDdns": {
    "ip-address": "127.0.0.1",
    "port": 53001,
    "forward-ddns": {
      "ddns-domains": [{
        "name": "example.com.",
        "key-name": "ddns-key",
        "dns-servers": [{
          "ip-address": "127.0.0.1",
          "port": 53
        }]
      }]
    },
    "tsig-keys": [{
      "name": "ddns-key",
      "algorithm": "hmac-sha256",
      "secret": "<base64-secret-aus-ddns.key>"
    }]
  }
}
```

### 4. KEA DHCPv4 aktivieren (`kea-dhcp4.conf`)
```json
{
  "Dhcp4": {
    "dhcp-ddns": {
      "enable-updates": true
    },
    "ddns-qualifying-suffix": "example.com"
  }
}
```

### 5. Services starten
```bash
systemctl start kea-dhcp-ddns
systemctl start kea-dhcp4
systemctl restart bind9
```

Wichtig: Zone-Datei muss für BIND schreibbar sein und TSIG-Secret muss identisch sein.



