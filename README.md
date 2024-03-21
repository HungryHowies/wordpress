# wordpress
Web Server

Follow this guid
https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-on-ubuntu-22-04-with-a-lamp-stack 
adduser sammy
usermod -aG sudo sammy

Provide the FTP credentials inside /wp-config.php file 
```
define( 'FTP_USER', 'username' );
define( 'FTP_PASS', 'password' );
define( 'FTP_HOST', 'ftp.example.com:21' );
```
