# wordpress
Web Server
adduser sammy
usermod -aG sudo sammy

Provide the FTP credentials inside /wp-config.php file :
define( 'FTP_USER', 'username' );
define( 'FTP_PASS', 'password' );
define( 'FTP_HOST', 'ftp.example.com:21' );
