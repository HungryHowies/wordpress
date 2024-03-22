# Wordpress Setup

This Documentation is basic setup for Wordpress using Let's Encrypt. 
Prerequisite:

* Ubuntu 22.0.4
* All updates/Upgrades are completed.
* Network configured.
* DNS/FQDN is set. 
  

Before Setting up WordPress, LAMP stack steps need to be executed and once completed steps for TLS/SSL certificates need to be install/configured.

### Install Install Linux, Apache, MySQL, PHP (LAMP).

Steps are found [here](https://github.com/HungryHowies/wordpress/blob/b93e3dcff49dbe93bf39dc9dfaa9a1ff5b7f990b/lamp_setup.md)

### Secure Apache with Let's Encrypt

Steps are found [here](https://github.com/HungryHowies/wordpress/blob/6a573c7283b7b85ce81a91060fdc3a63c3011387/certificates_lets_encrypt.md)

## Creating a MySQL Database and User for WordPress

log into the MySQL root (administrative) account by issuing the following command.

```
mysql -u root -p
```

Create a dedicated database for WordPress to control.

```
CREATE DATABASE wordpress DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
```

Create a user by running the following command.

```
CREATE USER 'wordpressuser'@'%' IDENTIFIED  BY 'password';
```

These commands will let the database know that your wordpressuser should have complete access to the database.

```
GRANT ALL ON wordpress.* TO 'wordpressuser'@'%';
```
```
FLUSH PRIVILEGES;
```
```
EXIT;
```

## Installing Additional PHP Extensions

When LAMP was installed it only required minimal set of extensions in order to get PHP to communicate with MySQL. 

In this step PHP extensions are installed for use with WordPress.

```
 apt install php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip
```

Restart  Apache serivce.

```
sudo systemctl restart apache2
```

##  Adjusting Apache’s Configuration to Allow for *.htaccess* Overrides and Rewrites

To allow *.htaccess* files, you need to set the *AllowOverride* directive within a "Directory" block pointing to your document root. Add the following content inside the VirtualHost block in your configuration file, making sure to use the correct web root directory:

Edit file.

```
vi  /etc/apache2/sites-available/wordpress.conf
```
```
<VirtualHost *:80>
. . .
    <Directory /var/www/wordpress/>
        AllowOverride All
    </Directory>
. . .
</VirtualHost>
```

The end results should look lke this.

```
VirtualHost *:80>
    ServerName howie.hungry-howard.com
    ServerAlias www.howie.hungry-howard.com
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/wordpress
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
    <Directory /var/www/wordpress/>
        AllowOverride All
    </Directory>
RewriteEngine on
RewriteCond %{SERVER_NAME} =howie.hungry-howard.com [OR]
RewriteCond %{SERVER_NAME} =www.howie.hungry-howard.com
RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>
```

### Enabling the Rewrite Module

```
sudo a2enmod rewrite
```

Test the configurations.

```
sudo apache2ctl configtest
```

## Downloading WordPress

First, change into a writable directory and then download and set up WordPress.

Change to the TMP directory.

```
cd /tmp
```

Then download the compressed release with the following curl command.

```
curl -O https://wordpress.org/latest.tar.gz
```
```
tar xzvf latest.tar.gz
```

Move these files into your document root momentarily. Before doing so, you can add a dummy *.htaccess* file so that this will be available for WordPress to use later.

```
touch /tmp/wordpress/.htaccess
```

Copy the sample configuration PHP file.

```
cp /tmp/wordpress/wp-config-sample.php /tmp/wordpress/wp-config.php
```

Create the "Upgrade" directory so that WordPress won’t run into permissions issues when trying to do this on its own following an update to its software.

```
mkdir /tmp/wordpress/wp-content/upgrade
```

Copy the entire contents of the directory into your document root.

```
sudo cp -a /tmp/wordpress/. /var/www/wordpress
```

## Configuring the WordPress Directory

Start by giving ownership of all the files to the www-data user and group. This is the user that the Apache web server runs as, and Apache will need to be able to read and write WordPress files in order to serve the website and perform automatic updates.

```
 chown -R www-data:www-data /var/www/wordpress
```

The *find* commands to set the correct permissions on the WordPress directories and files.

```
sudo find /var/www/wordpress/ -type d -exec chmod 750 {} \;
```

This command finds each file within the directory and sets their permissions to 640.

```
sudo find /var/www/wordpress/ -type f -exec chmod 640 {} \;
```

### Setting Up the WordPress Configuration File

The first task will be to adjust some secret keys to provide a level of security for your installation.
To grab secure values from the WordPress secret key generator, run the following.

```
curl -s https://api.wordpress.org/secret-key/1.1/salt/
```

You will receive unique values. Copy and save these value to adjust Worpress configuration file in the next following steps.

Next, open the WordPress configuration file.

```
sudo nano /var/www/wordpress/wp-config.php
```

Find the section that contains the example values for those settings.

```
. . .

define('AUTH_KEY',         'put your unique phrase here');
define('SECURE_AUTH_KEY',  'put your unique phrase here');
define('LOGGED_IN_KEY',    'put your unique phrase here');
define('NONCE_KEY',        'put your unique phrase here');
define('AUTH_SALT',        'put your unique phrase here');
define('SECURE_AUTH_SALT', 'put your unique phrase here');
define('LOGGED_IN_SALT',   'put your unique phrase here');
define('NONCE_SALT',       'put your unique phrase here');

. . .
```

Delete those lines and insert the values you copied from the command line using the secret key generator from the step above.

Next, modify some of the database connection settings at the beginning of the file.

```
// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress' );

/** MySQL database username */
define( 'DB_USER', 'wordpressuser' );

/** MySQL database password */
define( 'DB_PASSWORD', 'password' );

/** MySQL hostname */
define( 'DB_HOST', 'localhost' );

/** Database Charset to use in creating database tables. */
define( 'DB_CHARSET', 'utf8' );

/** The Database Collate type. Don't change this if in doubt. */
define( 'DB_COLLATE', '' );
```

Save and close the file when you are finished.

## Completing the Installation Through the Web Interface

Either enter your IP Address or FQDN.

```
 https://wordpress.domian.com
```



