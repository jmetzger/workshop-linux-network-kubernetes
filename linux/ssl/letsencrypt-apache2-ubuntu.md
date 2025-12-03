# Letsencrypt apache2 auf Ubuntu 

## Step 0:

```
apt update
apt install apache2 -y
a2enmod ssl
systemctl restart apache2 
```

## Step 0.5: 

```
test im browser
app.tln<tlnnr>.t3isp.de
```


## Step 1: setup virtual host 


```
cd /etc/apache2/sites-available/
nano backend.tln<tlnnr>.t3isp.de.conf 
```

```
<VirtualHost *:80>
    ServerName backend.tln<tlnnr>.t3isp.de
    ServerAlias alternative.tln<tlnnr>.t3isp.de
    DocumentRoot /var/www/backend.tln<tlnnr>.t3isp.de/html
    ErrorLog /var/log/apache2/backend-tln<tlnnr>-t3isp-de-error.log
    CustomLog /var/log/apache2/backend-tln<tlnnr>-t3isp-de-access.log combined
</VirtualHost>
```

```
cd /var/www
mkdir -p backend.tln<tlnr>.t3isp.de/html
cd backend.tln<tlnnr>.t3isp.de/html/
echo "ich bin backend von jochen" > index.html
```

```
a2ensite  backend.tln<tln1>.t3isp.de
systemctl reload apache2 
```

```
# Im browser testen
```



## Install certbot

```
apt install -y certbot python3-certbot-apache
```

## Use certbot to configure 

```
certbot --apache --register-unsafely-without-email 
# gleich mit domains
# dann wird man nicht mehr gefragt 
# certbot --apache --register-unsafely-without-email -d backend.tln1.t3isp.de 

```

## Test with your browser 

```
https://backend.tln<tlnnr>.t3isp.de 

```

## Test Certificate with ssl labs 

  * https://ssllabs.com
  * https://github.com/testssl/testssl.sh

## Refs:

  * https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-centos-8
