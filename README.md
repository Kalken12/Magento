### Create Aws Instance for Debian 11
 - Debian 11 dont have  php8.1 version. It is having most stable version of php is php7.4 .
 - For php 8.1 we have to go on https://deb.sury.org/ this is debian php package page. 
 - packages.sury.org/php/README.txt open this txt file
    and follow the steps
- Update system
      `apt-get update`
- install following packages
- download https://packages.sury.org/debsuryorg-archive-keyring.deb this file and /tmp/debsuryorg-archive-keyring.deb on this location.
  `curl -sSLo /tmp/debsuryorg-archive-keyring.deb  https://packages.sury.org/debsuryorg-archive-keyring.deb`
-  install using dpkg to file at location /tmp/debsuryorg-archive-keyring.deb 
    `dpkg -i /tmp/debsuryorg-archive-keyring.deb`
-  use this command to copy data inside /etc/apt/sources.list/php.list
 ` sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'`
- you can check data in php.list
 `  cat /etc/apt/sources.list.d/php.list `
  `apt-get update`
- we have all drivers for php8.1 install now php8.1
  `apt install php8.1`
 `php -version`
- install some dependecies for maganto installation
  `apt install php8.1-xml  php8.1-curl php8.1-gd php8.1-intl php8.1-mysql php8.1-zip  php8.1-soap php8.1-mbstring php8.1-bcmath`

### Install Mysql
  - Install mysql
 `apt install ./mysql-apt-config_0.8.26-1_all.deb`
 #### correct the keyring data for the mysql archive.


### Install php8.1 on the Debian 11

