# Install bind9 on Ubuntu 

  * Achtung, die Reihenfolge ist wichtig !

## Schritt 1: Bind 9 installieren 

```
apt update
apt install bind9 bind9utils bind9-doc
systemctl status bind9 
```



## Step 2: Disable systemd-resolve  

```
systemctl disable systemd-resolved
systemctl stop systemd-resolved
# Delete symbolic Link 
rm -fR /etc/resolv.conf
```


## Step 3: Change settings in /etc/bind/named.conf.options 

```
// google public dns-server
forwarders {
   8.8.8.8;
   8.8.4.4;
};

listen-on { any; };

// diese Zeile ändern dnssec-validation auto -> in
dnssec-validation yes;
```

## Step 4: Edit resolv.conf


```
nameserver 127.0.01
```

## Step 5: Restart named 

```
systemctl stop named
hostnamectl set-hostname ns1.training.local 
systemctl start named
```

## Step 6: Test with dig 

```
dig @localhost A www.google.de
```

## Step 7: Here are the logs 

```
journalctl -eu named
```


## Step 8: Setup a zone 

```
# Zone in der Datei /etc/bind/named.conf.local hinzufügen
zone "training.local" {
    type master;
    file "/etc/bind/db.training.local";
};
```



```
; put into file /etc/bind/db.training.local 
; base zone file for training.local
$TTL 1h    ; default TTL for zone
$ORIGIN training.local. ; base domain-name
; Start of Authority RR defining the key characteristics of the zone (domain)
@         IN      SOA   ns1.training.local. hostmaster.training.local. (
                                2025120101 ; serial number
                                12h        ; refresh
                                15m        ; update retry
                                3w         ; expiry
                                2h         ; minimum
                                )
; name server RR for the domain
           IN      NS      ns1.training.local.
ns1        IN      A       192.168.56.101
ubuntu22   IN      A       192.168.56.101
```

```
systemctl restart named
```
