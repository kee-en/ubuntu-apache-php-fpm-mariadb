# ubuntu-apache-php-fpm-mariadb
How to Install Apache2, PHP-FPM &amp; Mariadb on Ubuntu for Local Development

Step1: Update your Ubuntu
```
$ sudo apt update
$ sudo apt upgrade
$ sudo apt dist-upgrade
```

Step2: Install Apache Web Server

```
$ sudo apt install apache2 apache2-utils
$ sudo systemctl start apache2
$ sudo systemctl enable apache2
```
> Move the owner of your web folder to Apache user
```
$ sudo chown www-data /var/www/html -R
$ sudo chmod -R 777 /var/www/html
```

Step3: Install MariaDB
```
$ sudo apt install mariadb-server mariadb-client
$ sudo systemctl start mariadb
$ sudo systemctl enable mariadb
```
> Now run the post installation security script.
> This settings is for local development only.
```
$ sudo mysql_secure_installation
```
```
Enter current password for root: Press Enter
Set root password? n
Remove anonymous users? Y
Disallow root login remotely? Y
Remove test database and access to it? Y
Reload privilege tables now? Y
```
Step4: Install PHP
```
$ sudo apt install php-fpm php-mysql php-common php-gd php-json php-cli php-curl libapache2-mod-php
$ sudo systemctl start php7-fpm
$ sudo systemctl enable php7-fpm
$ sudo a2enmod proxy_fcgi
```
> Then edit the virtual host configuration file. This tutorial uses the default virtual host as an example.
```
sudo nano /etc/apache2/sites-available/000-default.conf
```
> Add the ProxyPassMatch directive to this file.

```
....

ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
ProxyPassMatch ^/(.*\.php(/.*)?)$ unix:/run/php/php7.0-fpm.sock|fcgi://localhost/var/www/html/

.....
```
> Save and close this file. Restart Apache2.
```
sudo systemctl restart apache2
sudo systemctl restart php7-fpm
```
