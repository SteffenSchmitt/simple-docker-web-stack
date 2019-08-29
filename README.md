# Simple docker-compose web development environment
## Installation

1. ch ${PROJECT_ROOT}
2. ln -ls ./environment/.env.dist .env
3. ln -ls docker-compose.{YOUR_DESIRED_CONFIG}.yml docker-compose.yml
4. ln -ls docker-sync.{YOUR_DESIRED_SYNC_STRATEGY}.yml docker-sync.yml
5. docker-sync start
6. docker-compose up -d

For example:

```shell script
cd project

ln -ls ./environment/.env.dist .env \
ln -ls ./docker-sync.unison .docker-sync.yml \
ln -ls ./docker-compose.local.yml docker-compose.yml

docker-sync start && \
docker-compose up -d 
```

## Configuration

All you need to do is to edit your .env file to fit your needs. 

   You can Change Versions for PHP and Apache and Maria-DB as well as the default access-data,  sync-locations and container-names

> Defaults:
```shell script
# Version Constraints
PHP_VERSION=7.3
MARIADB_VERSION=10.3
APACHE_VERSION=2.4

# Container Service-Names
PHP_NAME=app
MARIADB_NAME=db
APACHE_NAME=web
ADMINER_NAME=adminer

# DB Parameters
DB_ROOT_PASSWORD=asdqwe123
DB_NAME=default
DB_USERNAME=admin
DB_PASSWORD=asdqwe123

# Misc Parameters
REMOTE_LOCATION_WEB_SYNCFOLDER=/var/www/html/
REMOTE_LOCATION_MARIADB_SYNCFOLDER=/var/lib/mysql

```

### V-Hosts
Your default v-host config for apache is located under: ```./environment/apache/default.vhost.apache.conf```

Default config:

```apacheconfig
ServerName localhost
LoadModule deflate_module /usr/local/apache2/modules/mod_deflate.so
LoadModule proxy_module /usr/local/apache2/modules/mod_proxy.so
LoadModule proxy_fcgi_module /usr/local/apache2/modules/mod_proxy_fcgi.so
<VirtualHost *:80>
  # Proxy .php requests to port 9000 of the php-fpm container
  ProxyPassMatch ^/(.*\.php(/.*)?)$ fcgi://php:9000/var/www/html/$1
  DocumentRoot /var/www/html/
  <Directory /var/www/html/>
    DirectoryIndex index.php
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
  </Directory>
  # Send apache logs to stdout and stderr
  CustomLog /proc/self/fd/1 common
  ErrorLog /proc/self/fd/2
</VirtualHost>
```
## Sync Settings
By default all your database is synced to ```.data/```

Your webserver contents are located in ```webroot/```
