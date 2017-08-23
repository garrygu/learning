## Folder Structure
- http://devdocs.magento.com/guides/v2.0/cloud/access-acct/first-time-setup_dir-structure.html
- https://www.siteground.com/tutorials/magento/structure.htm


## Environment



### PHP
- Get PHP version  
`php -i` or `php -v`  
`rpm -q php`  
`more /etc/php.ini`  
`yum list installed | grep -i php`  
`yum info php56u`  
`which php`

- Remove php  
`yum remove php-*`

- Install PHP 7 on Centos 7  
`yum install -y http://dl.iuscommunity.org/pub/ius/stable/CentOS/7/x86_64/ius-release-1.0-14.ius.centos7.noarch.rpm`  
`yum -y update`  
`yum -y install php70u php70u-pdo php70u-mysqlnd php70u-opcache php70u-xml php70u-mcrypt php70u-gd php70u-devel php70u-mysql php70u-intl php70u-mbstring php70u-bcmath php70u-json php70u-iconv`

  - Restart Apache  
`service httpd restart`

#### Required PHP settings
-  Find the PHP configuration file, `php.ini`  
  *  Run a phpinfo.php file:   
  http://localhost/phpinfo.php  mysql
  ```
  <?php
  // Show all information, defaults to INFO_ALL
  phpinfo();
  ?>
  ```
  * To locate the PHP command-line configuration  
  `php --ini`  
  Locate: `Loaded Configuration File:         /etc/php.ini`

- To find OPcache configuration settings
--versionyum -y   * Apache web server
  For CentOS with Apache or nginx, OPcache settings are typically located in `/etc/php.d/opcache.ini`  
  Or `sudo find / -name 'opcache.ini'`


### MySQL
http://www.tecmint.com/install-latest-mysql-on-rhel-centos-and-fedora/

- Check MySQL version
```
mysql --version
mysql> select version();
mysql> SHOW VARIABLES LIKE “%version%”;
rpm -qa | grep mysql
```

- Get MySQL 5.7 for CentOS 7
```
wget http://dev.mysql.com/get/mysql57-community-release-el7-7.noarch.rpm
yum -y localinstall mysql57-community-release-el7-7.noarch.rpm
```

- Install and configure MySQL  
```
yum -y install mysql-community-server
service mysqld start  or: sudo systemctl start mysqld.service  
service mysqld status
##
## Enter the following command to get the temporary database root user password:
grep 'temporary password' /var/log/mysqld.log
## Secure the installation
mysql_secure_installation
mysql -u root -p
```

- Enable mysql auto start at boot time:  
`systemctl enable mysqld.service`


- Config file
```
/etc/my.cnf:  
  datadir=/var/lib/mysql  
  socket=/var/lib/mysql/mysql.sock  :q

  log-error=/var/log/mysqld.log  
  pid-file=/var/run/mysqld/mysqld.pid  
```

- Change the default password plugin level
`SET GLOBAL validate_password_policy=LOW;`

- Remove the validate password plugin
`uninstall plugin validate_password;`


#### Configuring the Magento database instance
```
create database magento;
GRANT ALL ON magento.* TO magento@localhost IDENTIFIED BY 'magento';
##
## Verify the database:
mysql -u magento -p
```

### Install Composer
```
curl -sS https://getcomposer.org/installer | php
sudo chmod 755 composer.phar
mv composer.phar /usr/local/bin/composer
```


## Get your authentication keys
