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


## Step 3: Change settings in /etc/bin/named.conf.options 

```
// google public dns-server 
forwarders {
   8.8.8.8;
   8.8.4.4;
}

listen-on { any; };

// diese Zeile ändern dnssec-validation auto -> in
dnssec-enable: yes
dnssec-validation: yes;

```

## STep 4: Edit resolv.conf


```
nameserver 127.0.01
```
