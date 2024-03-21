# Apache with Let's Encrypt on Ubuntu 22.04

### Installation Let's encrypt
You need two packages: certbot, and python3-certbot-apache.
```
 apt install certbot python3-certbot-apache
```

Checking your Apache Virtual Host Configuration.

```
vi /etc/apache2/sites-available/your_domain.conf
```
Find the existing ServerName and ServerAlias lines. They should be listed as follows:
```
ServerName your_domain
ServerAlias www.your_domain
```
test configurations
```
 apache2ctl configtest
```
```
sudo systemctl reload apache2
```
### Obtaining an SSL Certificate

This script will prompt you to answer a series of questions in order to configure your SSL certificate.

```
sudo certbot --apache
```

