# Letsencrypt apache2 auf Ubuntu 

# Apache SSL with letsencrypt ssl 

## Step 0:

```
apt update
apt install apache2
a2enmod ssl
systemctl restart apache2 


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
    ErrorLog /var/log/httpd/backend-tln<tlnnr>-t3isp-de-error.log
    CustomLog /var/log/httpd/bakend-tln<tlnnr>-t3isp-de-access.log combined
</VirtualHost>
```

```
/var/www
mkdir -p backend.tln<tlnr>.t3isp.de/html
cd ap1.t3isp.de/html/
echo "ich bin backend von jochen" > index.html
```

```
a2ensite  backend.tln<tln1>.t3isp.de
```

```
# Im browser testen
```



## Install certbot (Centos 8) 

```
dnf install -y epel-release 
dnf install -y certbot python3-certbot-apache mod_ssl
```

## Variant 1: Attention: virtual host - domain must be different than hostname 

```
e.g. ap1.t3isp.de (virtual host domain) != hostname 
# if this is not the case change hostname
hostnamectl set-hostname main.training.local 

# be sure to restart apache to take 
systemctl restart httpd 

```

## Variant 2: Remove <VirtualHost> </VirtualHost> - block vom ssl.conf 

```
# ssl.conf - Created by installation of mod_ssl. 
cd /etc/httpd/conf.d/
# vi ssl.conf 
remove
<VirtualHost>
....
</VirtualHost>

systemctl restart httpd 

```

## Use certbot to configure 

```
certbot --apache --register-unsafely-without-email 
```

## Test with your browser 

```
https://ap1.t3isp.de 

```

## Test Certificate with ssl labs 

  * https://ssllabs.com

## Refs:

  * https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-centos-8
