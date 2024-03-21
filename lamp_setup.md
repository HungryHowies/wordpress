# Lamp Basic Setup

This Documentation is basic setup for LAMP. 

* Ubuntu 22.0.4
* All updates/Upgrade are completed
* 

### Install Install Linux, Apache, MySQL, PHP (LAMP).

Install Apache Server

```
sudo apt install apache2
```

Install MariaDb Server
```
sudo apt install mariadb-server
```
Database settings.

```
mysql_secure_installation
```
Dependnacies

```
sudo apt install php libapache2-mod-php php-mysql
```


Check PHP version
```
php -v
```
### Changing Apache’s Directory Index (Optional)

I found out later this did help.

Edit  file 

```
vi /etc/apache2/mods-enabled/dir.conf
```

Need to make adjustments.

It should look like this.

```
<IfModule mod_dir.c>
    DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
</IfModule>
```

Move the PHP index file (index.php) to the first position after the DirectoryIndex specification.

Results.
```
<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```
Restart Apache service.
```
systemctl restart apache2
```
Check status
```
systemctl status apache2
```

### Creating a Virtual Host for your Website.

```
mkdir /var/www/your_domain
```
```
 chown -R $USER:$USER /var/www/your_domain
```

Create configuration file for site.
```
vi /etc/apache2/sites-available/your_domain.conf
```

Add the folloing with adjustments.
```

<VirtualHost *:80>
    ServerName your_domain
    ServerAlias www.your_domain
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/your_domain
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
enable site
```
a2ensite your_domain
```
Disable the defualt site
```
sudo a2dissite 000-default
```
Test site file.
```
sudo apache2ctl configtest
```
Once compelted reload apache.
```
sudo systemctl reload apache2
```















Follow this guid
https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-on-ubuntu-22-04-with-a-lamp-stack 


