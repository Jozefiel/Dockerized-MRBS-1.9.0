
# Dockerized Meeting Room Booking System 1.9.0

In this repository is dockerized Meeting Room Booking System v 1.9.0 from https://mrbs.sourceforge.io/.

# Before first run!
Is need to prepare **www/config.inc.php** and **docker-compose.yaml** file. 

In docker-compose.yaml file can be changed default settings:
  - ports in www service: define url for connecting to web page 
	  - for example <http:127.0.0.1:8080>
  - links in www service: define network hostname.
	  - Must be same as **$db_host** parameter in config.inc.php with prefix
  - MYSQL_DATABASE: define name of created mysql database.
	  -  Must be same as **$db_database** parameter in config.inc.php
  - MYSQL_USER: define name of created mysql user. 
	  -  Must be same as **$db_login** parameter in config.inc.php
  - MYSQL_PASSWORD: define password of created mysql user. 
	  - Must be same as **$db_password** parameter in config.inc.php
  - MYSQL_ROOT_PASSWORD: define password of created mysql root user.

```yaml
version: "3.1"
services:
    www:
        build:
          context: .
        network_mode: "host"
        image: ubuntu_mrbs:latest
        ports: 
            - "8080:80"
        volumes:
            - ./www:/var/www/html/
        links:
            - db:docker_host
    db:
        image: mysql:8.0
        ports: 
            - "3306:3306"
        command: --default-authentication-plugin=mysql_native_password
        environment:
            MYSQL_DATABASE: mrbs
            MYSQL_USER: mrbs
            MYSQL_PASSWORD: mrbs-password
            MYSQL_ROOT_PASSWORD: root-password 
        volumes:
            - ./init_db:/docker-entrypoint-initdb.d
            - mrbs_db:/var/lib/mysql
volumes:
    mrbs_db:

```

This is minimal settings in config file, for more setting please visit [https://github.com/yorkulibraries/mrbs]

```php
// config.inc.php minimal settings
<?php // -*-mode: PHP; coding:utf-8;-*-
namespace MRBS;

$timezone = "Europe/Bratislava";  // Set your timezone
$dbsys = "mysql";
$db_host = "docker_host";  // Defined in docker-compose file
$db_database = "mrbs"; // Defined in docker-compose file
$db_login = "mrbs"; // Defined in docker-compose file
$db_password = 'mrbs-password'; // Defined in docker-compose file
$db_tbl_prefix = "mrbs_";
$db_persist = FALSE;
```

# How to run docker-compose!

```sh
git clone https://github.com/Jozefiel/Dockerized-MRBS-1.9.0.git
cd Dockerized-MRBS-1.9.0.git
# run service in detached mode
docker-compose up -d 
# stop service
docker-compose down 
# stop service and clean mysql database
docker-compose down -v
```

### Another versions

 - Download another version of MRBS
 - Move database sql file to init_db (tables.my.sql or upgrade.my.sql)
 - Move web files into www folder

License
----
For docker files - MIT
For MRBS read LICENSE file

[https://github.com/yorkulibraries/mrbs]: <https://github.com/yorkulibraries/mrbs/blob/master/web/systemdefaults.inc.php>

