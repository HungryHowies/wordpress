# Apache with Let's Encrypt on Ubuntu 22.04

### Installation Let's encrypt

You need two packages: certbot, and python3-certbot-apache.

```
 apt install certbot python3-certbot-apache
```

Checking Apache Virtual Host Configuration.

```
vi /etc/apache2/sites-available/your_domain.conf
```

Check to ensure the existing ServerName and ServerAlias lines are correct. They should be listed as follows.

```
ServerName your_domain
ServerAlias www.your_domain
```

Test the configurations.

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
If any errors shown they should be correct before moving on to install Wordpress.
