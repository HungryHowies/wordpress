# SSO Plugin and Cache

 The following documentaion show what plugin to use and  how to configure Cache.

Install Plugin
* miniOrange SSO using SAML 2.0
fill out the requirements

## Cacheing
edit file
```
vi /etc/php/8.1/apache2/php.ini
```
Adjust these lines
```
opcache.enable=1
opcache.memory_consumption=128
opcache.max_accelerated_files=10000
opcache.revalidate_freq=200
```
Restart service
```
systemctl restart apache2
```

Install the following packages.

```
sudo apt-get install -y php8.1-fpm
```
```
sudo apt-get install -y imagemagick
```
```
sudo apt-get install -y php-imagick
```

Load imagick  for PHP.

```
sudo phpenmod imagick
```

Restart these services.

```
systemctl restart php8.1-fpm
```
```
systemctl restart apache2
```
