# Lamp Basic Setup

This Documentation is basic setup for LAMP. This is the preperation for Wordpress.

### Install Install Linux, Apache, MySQL, PHP (LAMP).

Install Apache Server.

```
sudo apt install apache2
```

Install MariaDb Server.

```
sudo apt install mariadb-server
```

MySQL Database settings.

```
mysql_secure_installation
```

Install the required Dependnacies.

```
sudo apt install php libapache2-mod-php php-mysql
```

Check PHP version.

```
php -v
```

### Changing Apache’s Directory Index (Optional)

I found out later this did help. Edit the following file.

```
vi /etc/apache2/mods-enabled/dir.conf
```

Some adjustments need to be made in the file. The order in which Apache2 will follow as shown below.

It should look like this.

```
<IfModule mod_dir.c>
    DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
</IfModule>
```

Move the PHP index file (index.php) to the first position after the DirectoryIndex specification.

Results:

```
<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```

Restart Apache service.

```
systemctl restart apache2
```

Check Apache service status.

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

Enable the custom site.

```
a2ensite your_domain
```

Disable the defualt site.

```
sudo a2dissite 000-default
```

This will ensure the costum site  is ready.

```
sudo apache2ctl configtest
```

Once compelted reload apache.

```
sudo systemctl reload apache2
```
