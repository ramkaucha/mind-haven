---
title: 
tags: 
cssclasses: 
author:
---
Demo moodle:

Using AWS Lightsail since i don't need that much processing power for my demo.

Create an instance of lightsail
Download the SSH key and give me write permission
`chmod 400 /path/to/adad.peb`
ssh into the instance
`ssh -i /path/to/adad.peb ubuntu@IPv4`

`export TERM=xterm`

`sudo apt update && sudo apt upgrade -y sudo apt install docker.io docker-compose git -y`

clone moodle, and moodle-docker
directory structure:
```
/project/
|-- moodle/
|-- moodle-docker/
```

create config file
`cp moode/config-dist.php moodle/config.php`

create environment file
`cp moodle-docker/config.docker-template.php config.docker.php`

setup env variables 
```bash
export MOODLE_DOCKER_WWWROOT=/path/to/moodle
export MOODLE_DOCKER_DB=pgsql
```

start containers
`bin/moodle-docker-compose up -d`

Install moodle (script)
```bash
#!/bin/bash

# install moodle database
SITE_FULLNAME="Site name"
SITE_SHORTNAME="Site short name"
ADMIN_PASSWORD="password"
ADMIN_EMAIL="email@example.com"

# database configuration - default moodle-docker settings
DB_TYPE="pgsql"
DB_HOST="pgsql"
DB_NAME="moodle"
DB_USER="moodle"
DB_PASS="m@0dl3ing"

# running installation command
bin/moodle-docker-compose exec webserver php admin/cli/install_database.php \
--agree-license \
--fullname="$SITE_FULLNAME" \
--shortname="$SITE_SHORTNAME" \
--adminpass="$ADMIN_PASSWORD" \
--adminemail="$ADMIN_EMAIL" \
--dbtype="$DB_TYPE" \
--dbhost="$DB_HOST" \
--dbname="$DB_NAME" \
--dbuser="$DB_USER" \
--dbpass="$DB_PASS"
```
save as `install.sh`
exec: `chmod +x install.sh`
run `./isntall.sh`

Checking docker contains (if any are running): `docker ps -a`

