## Create Aws Instance for Debian 11

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

### keyring data for the mysql archive.
` mkdir /etc/apt/keyrings`
`gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys B7B3B788A8D3785C && rm 
-f /usr/share/keyrings/mysql-apt-config.gpg && gpg  --output /usr/share/keyrings/mysql-apt-config.gpg --export B7B3B788A8D3785C`


### Install Mysql
  
 `apt install ./mysql-apt-config_0.8.26-1_all.deb`
 - Install mysql-server (check using   apt search mysql-server)
   `apt install mysql-server`
 - check version
  `mysql --version`
### Install Nginx
- Install Nginx
  `apt install –no-install-recommends nginx`
- Check status 
  'systemctl status nginx'
### install Elasticsearch and configuration needs prerequisite
  - update system
  `apt update`
#### prerequisite:
   - install openjdk default
`apt install –no-install-recommends default-jre`
 - To begin, use cURL, the command line tool for transferring data with URLs, to import the Elasticsearch public GPG key into APT. Note that we are using the arguments -fsSL to silence all progress and possible errors (except for a server failure) and to allow cURL to make a request on a new location if redirected. Pipe the output of the cURL command into the apt-key program, which adds the public GPG key to APT.
`curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -`
- Next, add the Elastic source list to the sources.list.d directory, where APT will search for new sources:
`echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list`
- Next, update your package lists so APT will read the new Elastic source:
`sudo apt update`
### Install Elasticsearch


