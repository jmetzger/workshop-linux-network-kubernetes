# Beispiel für Reservations 

## Eintrag erfolgt in der /etc/kea/kea-dhcp4.conf 



```
// Diese an die Datei anfügen 
"reservations": [
    {
        "duid": "01:02:03:04:05:06:07:08:09:0A",

        "ip-addresses": [
            "192.168.0.10"
        ],
        "hostname": "foo.example.com"
    }
]
```
