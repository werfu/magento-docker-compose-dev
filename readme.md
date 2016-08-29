# Magento 1.9 development Docker-compose scaffold
This repo hold a docker-compose base configuration for Magento 1.9. It's based upon stock PHP-FPM, MySQL and Nginx image, and use minimal customization to these by using configuration injection. Only the FPM image is customized to install required PHP extensions. This is only for development purpose. It provide PHPMyAdmin and Mailcatcher as conveniance.  It's also unsafe, password are weaks and you will face problems if you try to use it in production. DO NO USE FOR LIVE SITE.

## Why use this instead of using (insert the of name your prefered Magento docker image here)
Most Magento Docker image are monolytic or at most using an external MySQL server. This may be great if you don't need to tailor the configuration often or like to rebuild image, but I prefer to be able to tweak things easily while at the same time rely on the thinest images possible. As such, it's quite easy to switch images version, add Varnish or Pound support or add a Solar server, without having to customize a Dockerfile. Docker-compose is a simpler, more elegant solution. Use it!

## Features
* Latest Nginx
* MySQL 5.5
* PHP-FPM 5.6 with the following extensions:
⋅⋅* XDebug
⋅⋅* gd (with freetype and jpeg support)
⋅⋅* iconv
⋅⋅* mcrypt
⋅⋅* curl
⋅⋅* dom
⋅⋅* hash
⋅⋅* pdo
⋅⋅* pdo_mysql
⋅⋅* simplexml
⋅⋅* soap
* Redis
* PHPMyAdmin (accessible on port 8080)
* MailCatcher (accessible on port 1080)

## Services structure
Docker-compose use service name as hostname. You will need those while configuring
* db: MySql server
* fpm: PHP-FPM. If you need to execute PHP commands (like running n98-magerun), do it there.
* redis: Redis cache
* mailcatcher: Mailcatcher is a fake SMTP service which catch all mail going through and allow you to read them in a web interface. Pretty usefull to debug mail template and suchs.
* phpmyadmin: PHPMyAdmin is configured to allow remote connection. Use the database credentials in the following section to log onto the db server.
* nginx: A plain nginx server.

## Database configuration
The MySQL database configuration are passed to the MySQL instance as environment variables set inside the docker-compose.yml file. They are used to initialize the MySQL instance on the first run. Feel free to adjust them as you need. By default the root password, the regular user name, it's password AND the database are all set to "magento". AGAIN, THERE IS NO SECURITY EXPECTATIONS HERE, DO NOT USE IN PRODUCTION.

## Folder structure
* www : Put your public Magento folder there. If you want your Magento site to be able to write to the folder, you'll have to ensure the group 33 is write-enabled (www-data on debian, http on arch) on the folder. New files will be created with the www-data user on the PHP-FPM machine.
* db/init : Any scripts (SQL or sh) placed there will be ran on the first machine build. Usefull for importing a database
* db/data : This is where the MySQL database are stored. You can copy your MySQL database straight there too (it might require some tweaks though, have a read at the MySQL docker image documentation).

## Configuration customization
* Nginx default site configuration is exposed in conf/site.conf. The configuration is straightfowared, with basic rewrites and no security at all.
* PHP configuration can be tailored by editing the fpm/custom.ini.
* FPM pool configuration is not exposed.

## PHP Debugging
Xdebug extension configuration is exposed in conf/xdebug.conf. By default the remote port is set to 9009 and the remote handler to dbgp. The connect_back option is enable, so ensure your IDE is allowing it.


