# Magento Configuration
### Create Aws Instance for Debian 11

 - Debian 11 dont have  php8.1 version. It is having most stable version of php is php7.4 .
 - For php 8.1 we have to go on https://deb.sury.org/ this is debian php package page. 
 - Packages.sury.org/php/README.txt open this txt file
    and follow the steps
- Update system
      `apt-get update`
- Install following packages
- Download https://packages.sury.org/debsuryorg-archive-keyring.deb this file and /tmp/debsuryorg-archive-keyring.deb on this location.
      `curl -sSLo /tmp/debsuryorg-archive-keyring.deb  https://packages.sury.org/debsuryorg-archive-keyring.deb`
-  install using dpkg to file at location /tmp/debsuryorg-archive-keyring.deb 
      `dpkg -i /tmp/debsuryorg-archive-keyring.deb`
-  Use this command to copy data inside /etc/apt/sources.list/php.list
      ` sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'`
- You can check data in php.list

      ` cat /etc/apt/sources.list.d/php.list `

      `apt-get update`

- we have all drivers for php8.1 install now php8.1

      `apt install php8.1`

      `php -version`

- install some dependecies for maganto installation

      `apt install php8.1-xml  php8.1-curl php8.1-gd php8.1-intl php8.1-mysql php8.1-zip  php8.1-soap php8.1-mbstring php8.1-bcmath`

### Keyring data for the mysql archive.

     `mkdir /etc/apt/keyrings`

     `gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys B7B3B788A8D3785C && rm -f /usr/share/keyrings/mysql-apt-config.gpg && gpg  --output /usr/share/keyrings/mysql-apt-config.gpg --export B7B3B788A8D3785C`


### Install Mysql
  
     `apt install ./mysql-apt-config_0.8.26-1_all.deb`
 - Install mysql-server (check using   apt search mysql-server)
     `apt install mysql-server`
 - Check version
     `mysql --version`
### Install Nginx
- Install Nginx
     `apt install –no-install-recommends nginx`
- Check status 
      'systemctl status nginx'
### Install Elasticsearch and configuration needs prerequisite
  - update system
      `apt update`
#### Prerequisite:
   - Install openjdk default
   
     `apt install –no-install-recommends default-jre`
 - To begin, use cURL, the command line tool for transferring data with URLs, to import the Elasticsearch public GPG key into APT. Note that we are using the arguments -fsSL to silence all progress and possible errors (except for a server failure) and to allow cURL to make a request on a new location if redirected. Pipe the output of the cURL command into the apt-key program, which adds the public GPG key to APT.

     `curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -`
- Next, add the Elastic source list to the sources.list.d directory, where APT will search for new sources:
     `echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list`

- Next, update your package lists so APT will read the new Elastic source:
     `sudo apt update`

### Install Elasticsearch

- Install elasticsearch by following command 
    
   ` apt install  --no-install-recommends elasticsearch`

- Elasticsearch listens for traffic from everywhere on port 9200. You will want to restrict outside access to your Elasticsearch instance to prevent outsiders from reading your data or shutting down your Elasticsearch cluster through its [REST API] . To restrict access and therefore increase security, find the line that specifies network.host, uncomment it, and replace its value with localhost so it reads like this
  "network.host: localhost"

- Start the Elasticsearch service with systemctl. Give Elasticsearch a few moments to start up. Otherwise, you may get errors about not being able to connect.

     'sudo systemctl start elasticsearch'

- Next, run the following command to enable Elasticsearch to start up every time your server boots:
    `sudo systemctl enable elasticsearch`

### Install Composer

     `curl -sS https://getcomposer.org/installer -o composer-setup.php`
     `sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer`

- You have successfully installed Composer on the system. It is available for global use as we have stored it to /usr/local/bin/.

- Check the Composer version with the following command -

     `composer -V`

### Installation and Configuration of Magento

#### create access key

  - To install Magento 2 using composer, you need to have an account at https://marketplace.magento.com/. We would need to create an access key and use it to install Magento through the command line. In the link above, you can navigate to My profile > Marketplace > My products > Access Keys to see/create an access key.To install Magento 2 using composer, you need to have an account at https://marketplace.magento.com/. We would need to create an access key and use it to install Magento through the command line. In the link above, you can navigate to My profile > Marketplace > My products > Access Keys to see/create an access key.

-  successfully we created key that key will be useful for further at time of installing magneto

-  Change the writting permission for the /opt folder

`$ sudo chmod o+w  /opt /`

`$ composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition /opt/ magento2`

- Enter the username and password, the keys are shown below.
    • Username: Public Key
    • Password: Private Key

- You will not see the password string. Simply paste it and composer will start downloading Magento 2.

#### Installation

- Magento files are downloaded to directory /opt/magento2. To use another path, you can edit the command above for another location.
After the download is complete, you can start the installation with the command below-

     `cd /opt/magento2`

#### Create Admin User

- You can edit the domain name, email address, and admin password. Use the following command -

    `php bin/magento setup:install \
--base-url=https://test.mgt.com  \
--db-host=localhost \
--db-name=magentodb \
--db-user=magento \
--db-password=magento \
--admin-firstname=kalyani \
--admin-lastname=kenekar \
--admin-email=admin@admin.com \
--admin-user=admin2 \
--admin-password=adminpass2 \
--language=en_US \
--currency=USD \
--timezone=America/Chicago \
--use-rewrites=1`

- After the installation is complete, you will see the following output:

[SUCCESS]: Magento installation complete. 
[SUCCESS]: Magento Admin URI: /admin_xyz


- You can correct the permissions with the command below -
     `$ sudo chown -R www-data. /opt/magento2`
- Two-factor authentication is enabled by default. If you want to disable it, you can run the command -

     `$ sudo -u www-data bin/magento module:disable Magento_TwoFactorAuth`

     `$ sudo -u www-data bin/magento cache:flush`


#### Setup Cron Jobs
- Magento uses cron jobs to automate its important system functions.
- Create the cron jobs with the following command -

`$ sudo -u www-data bin/magento cron:install`

- Making test.mgt.com accessible through /etc/hosts

`$ sudo  vi /etc/hosts`

 ```
   Ip_address_of_nginxhost   test.mgt.com
```
### Install and Configure Redis Server 
-Install Redis-Server and configure Magento to store the cache files and the sessions in Redis instead of the file system.

#### Install redis
  
   `apt install redis`
- check version
 
   `redis --v`
#### Configure Magento2 with Redis as Cache Storage
- To configure Redis for Magento 2 cache, replace the cache settings in /opt/magento2/app/etc/env.php with the following one:

    `sudo vi /opt/magento2/app/etc/env.php`

```
'cache' => [
    'frontend'
 => [
        'default' => [
            'backend' => 'Cm_Cache_Backend_Redis',
            'backend_options' => [
                'server' => '127.0.0.1',
                'port' => '6379',
                'database' => '0',
                'compress_data' => '1',
                'force_standalone' => '0',
                'connect_retries' => '10',
                'read_timeout' => '30',
                'automatic_cleaning_factor' => '0',
                'compress_tags' => '1',
                'compress_threshold' => '20480',
                'compression_lib' => 'gzip'
            ]
        ]
    ]
],

```

#### Configure Magento 2 to use Redis as session storage
- To configure Magento 2 to use Redis for sessions, replace the session settings in/opt/magento2/ app/etc/env.php with the following one:

 ```
'session' => [
    'save' => 'redis',
    'redis' => [
        'host' => '127.0.0.1',
        'port' => '6379',
        'password' => '',
        'timeout' => '15',
        'persistent_identifier' => '',
        'database' => '2',
        'compression_threshold' => '2048',
        'compression_library' => 'gzip',
        'log_level' => '1',
        'max_concurrency' => '15',
        'break_after_frontend' => '5',
        'break_after_adminhtml' => '30',
        'first_lifetime' => '600',
        'bot_first_lifetime' => '60',
        'bot_lifetime' => '7200',
        'disable_locking' => '0',
        'min_lifetime' => '60',
        'max_lifetime' => '2592000'
    ]
],

```

#### Configure Redis Default Caching

   `sudo -u www-data bin/magento setup:config:set --cache-backend=redis --cache-backend-redis-server=127.0.0.1 –cache-backend-redis-db=0`
 
### Install Magento Sample Data
   `vi  /opt/magento2/composer.json`
 
- paste above below data inside "require" field 

```
{
    "require": {
        "magento/module-bundle-sample-data": "100.4.*",
        "magento/module-catalog-rule-sample-data": "100.4.*",
        "magento/module-catalog-sample-data": "100.4.*",
        "magento/module-cms-sample-data": "100.4.*",
        "magento/module-configurable-sample-data": "100.4.*",
        "magento/module-customer-sample-data": "100.4.*",
        "magento/module-downloadable-sample-data": "100.4.*",
        "magento/module-grouped-product-sample-data": "100.4.*",
        "magento/module-msrp-sample-data": "100.4.*",
        "magento/module-offline-shipping-sample-data": "100.4.*",
        "magento/module-product-links-sample-data": "100.4.*",
        "magento/module-review-sample-data": "100.4.*",
        "magento/module-sales-rule-sample-data": "100.4.*",
        "magento/module-sales-sample-data": "100.4.*",
        "magento/module-swatches-sample-data": "100.4.*",
        "magento/module-tax-sample-data": "100.4.*",
        "magento/module-theme-sample-data": "100.4.*",
        "magento/module-widget-sample-data": "100.4.*",
        "magento/module-wishlist-sample-data": "100.4.*",
        "magento/sample-data-media": "100.4.*"
    }
}
```
- Update composer

    `sudo composer update`

- If prompted for a username and password, use your authentication keys.

   *Note - The public key is the username, and the private key is the password.*

- Verify the sample data installation by running the following commands:

     `sudo php bin/magento setup:upgrade`

     `sudo php bin/magento setup:di:compile

     `sudo php bin/magento setup:static-content:deploy -f`

- Now done with the sample data, You can check on browser by refreshing test.mgt.com data will be there.

### Self-Signed SSL Certificate test.mgt.com

 - create certificate

   `$ sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/magento2_selfsigned.key -out /etc/ssl/certs/magento2_selfsigned.crt`

- Generating a RSA private key

- writing new private key to '/etc/ssl/private/magento2_selfsigned.key'

 *Note: You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.*

```
Country Name (2 letter code) [AU]:DE
State or Province Name (full name) [Some-State]:Germany
Locality Name (eg, city) []:Berlin 
Organization Name (eg, company) [Internet Widgits Pty Ltd]:mgt.commerce
Organizational Unit Name (eg, section) []:IT-Department                                
Common Name (e.g. server FQDN or YOUR name) []:test.mgt.com
Email Address []:admin@test.mgt.com
```
- Change configuration file
   `sudo vi  /etc/nginx/conf.d/magento2.conf`

```
upstream fastcgi_backend {
  server  unix:/run/php/php8.1-fpm.sock;
}
server {
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/ssl/certs/magento2_selfsigned.crt; # 
    ssl_certificate_key /etc/ssl/private/magento2_selfsigned.key; # 
    include /etc/nginx/snippets/ssl-params.conf; 
    set $MAGE_ROOT /opt/magento2;
  include /opt/magento2/nginx.conf.sample;
}
server {
listen 8080;
server_name test.mgt.com;
  return 301 https://$host$request_uri;
  set $MAGE_ROOT /opt/magento2;
 include /opt/magento2/nginx.conf.sample;
}
```
- Restart Nginx Server

    `systemctl restart nginx`

- Access test.mgt.com with secure protocol
   *Note: It is self signed certificate thats why it may be look like Insecure.*

### Configure Varnish with Mangento

-  Go to admin panal using test.mgt.com/admin_panel
- If you dont know correct url for the admin_panel
     `php bin/magento info:adminuri`

- Give credential for admin user and password and redirect to admin panel.
- Go to this path store>configuration>advance>system>full page cache
- Add in cache  as "varnish cache"
- File default.vcl must be downloaded to your pc (*Note:check apropriate version*).
- Add port 80 for varnish and as per version 6 download file default.vcl
- Move file
    `mv /etc/varnish/default.vcl /etc/varnish/default.vcl.Back`

- Now upload default.vcl which was downloaded from your magento admin panel.
- Now we need to change the port. By default Varnish cache listening a :6081 port. But we need :80 port and nginx at :8080
     `vi /etc/nginx/conf.d/magento.conf`

```
server {
   listen 8080;
server_name test.mgt.com;   
set $MAGE_ROOT  /opt/magento; 
   set $MAGE_RUN_TYPE website;
   set $MAGE_MODE developer;
   include /var/www/yoursite/nginx.conf.sample;
}

```
- So first of all lets edit Varnish configuration.
`systemctl restart varnish`

    `systemctl restart nginx`



### Install and configure phpmyadmin with mangento
- Installing phpmyadmin



