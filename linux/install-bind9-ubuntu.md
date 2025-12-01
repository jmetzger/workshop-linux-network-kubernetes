# Install bind9 on Ubuntu 

## Step 1: Disable systemd-resolve  

```
systemctl disable systemd-resolved
systemctl stop systemd-resolved
# Delete symbolic Link 
rm -fR /etc/resolv.conf
```

```
apt update
apt install bind9 bind9utils bind9-doc
systemctl status bin9 
```
