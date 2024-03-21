# wordpress

This Documentation is basic setup for Wordpress using Let's Encrypt. 

* Ubuntu 22.0.4
* All updates/Upgrade are completed
* Network
* DNS
* 

Before Install WordPress a LAMP stack and secured with TLS/SSL certificates.

### Install Install Linux, Apache, MySQL, PHP (LAMP).

Steps are found [here](https://github.com/HungryHowies/wordpress/blob/b93e3dcff49dbe93bf39dc9dfaa9a1ff5b7f990b/lamp_setup.md)

### Secure Apache with Let's Encrypt

Steps are found [here](https://github.com/HungryHowies/wordpress/blob/6a573c7283b7b85ce81a91060fdc3a63c3011387/certificates_lets_encrypt.md)

## Creating a MySQL Database and User for WordPress

log into the MySQL root (administrative) account by issuing the following command.
```
mysql -u root -p
```
create a dedicated database for WordPress to control.
```
CREATE DATABASE wordpress DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
```
create this user by running the following command.
```
CREATE USER 'wordpressuser'@'%' IDENTIFIED  BY 'password';
```
let the database know that your wordpressuser should have complete access to the database you set up:
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

When install LAMP the require minimal set of extensions in order to get PHP to communicate with MySQL. 
PHP extensions for use with WordPress.
```
 apt install php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip
```
Restart  Apache serivce.
```
sudo systemctl restart apache2
```
##  Adjusting Apache’s Configuration to Allow for .htaccess Overrides and Rewrites

```




































Follow this guid
https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-on-ubuntu-22-04-with-a-lamp-stack 

