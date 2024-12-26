# Install IPAM Ubuntu 22.04
Install IPAM Ubuntu 22.04

1. Install mariadb
##
    sudo apt install mariadb-server mariadb-client -y
2. Start mariadb dan enable mariadb
##
    sudo systemctl enable mariadb
    sudo systemctl start mariadb
3. Buat Database
##
    sudo mysql -u root -p
##
    CREATE DATABASE phpipam;
##
    GRANT ALL ON phpipam.* TO phpipam@localhost IDENTIFIED BY
##
    GRANT ALL ON phpipam.* TO phpipam@localhost IDENTIFIED BY 'dbPassw0rd';
##
    FLUSH PRIVILEGES;
##
    QUIT;
4. Install PHP dan Module
##
    sudo apt -y install php php-{mysql,curl,gd,intl,pear,imap,memcache,pspell,tidy,xmlrpc,mbstring,gmp,json,xml,fpm}
5. Install GIT
##
    sudo apt install git -y
6. Clone php IPAM
##
    sudo git clone --recursive https://github.com/phpipam/phpipam.git /var/www/html/phpipam
7. Masuk ke directory
##
    cd /var/www/html/phpipam
8. Download php IPA<
##
    wget https://cyfuture.dl.sourceforge.net/project/phpipam/v1.7.3/phpipam-v1.7.3.tgz
9. Extract file php IPAM
##
    tar xvf phpipam-v1.7.3.tgz
10. Copy Folder hasil extract
##
    cp -r phpipam /var/www/html/
11. Edit file database
##
    nano config.php
##
    /**
    * database connection details
    ******************************/
    $db['host'] = 'localhost';
    $db['user'] = 'phpipam';
    $db['pass'] = 'dbPassw0rd';
    $db['name'] = 'phpipam';
    $db['port'] = 3306;
12. Install Apache2
##
    apt install apache2 -y
13. Konfigurasi apache2
##
    sudo a2dissite 000-default.conf
    sudo a2enmod rewrite
    sudo systemctl restart apache2
14. Install module php Apache2
##
    sudo apt -y install libapache2-mod-php php-curl php-xmlrpc php-intl php-gd
15. Buat konfigurasi ipam Apache2
##
    sudo nano /etc/apache2/sites-available/phpipam.conf
##
    <VirtualHost *:80>
    ServerAdmin admin@contoh.com
    DocumentRoot "/var/www/html/phpipam"
    ServerName ipam.contoh.com
    ServerAlias www.ipam.contoh.com
    <Directory "/var/www/html/phpipam">
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    ErrorLog "/var/log/apache2/phpipam-error_log"
    CustomLog "/var/log/apache2/phpipam-access_log" combined
    </VirtualHost>
16. Atur hak akses directory
##
    sudo chown -R www-data:www-data /var/www/html
17. Aktifkan Site
##
    sudo a2ensite phpipam
18. Restat Apache
##
    sudo systemctl restart apache2
19. Akses menggunakan browser
    http://IP.Server
# FINISH
