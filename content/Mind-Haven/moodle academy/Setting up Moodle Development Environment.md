Environment (10/12/24):
- Moodle docker
- PHP 8.1
- pgSQL
- Moodle 4.4


## Using docker to set up Moodle Development environment

Directory structure
```
project/
|-- moodle/ # moodle codebase
|-- moodle-docker/ # docker configuration
```

Docker:
`git clone https://github.com/moodlehq/moodle-docker.git`

Moodle Installation
```
cd /path/to/your/webroot  
git clone --branch MOODLE_404_STABLE https://github.com/moodle/moodle.git
```

Copy config files 
```
# inside moodle-docker
cp config.docker-template.php ../moodle/config.php
```

Create required scripts (every inside the docker folder)
```
# start-moodle.sh
export MOODLE_DOCKER_WWWROOT="/path/to/the/moodle"
export MOODLE_DOCKER_DBTYPE=pgsql
export MOODLE_DOCKER_DP=pgsql
export MOODLE_DOCKER_WEB_HOST=localhost

bin/moodle-docker-compose up -d
```

```
# stop-moodle.sh
export MOODLE_DOCKER_WWWROOT="/path/to/the/moodle"
export MOODLE_DOCKER_DBTYPE=pgsql
export MOODLE_DOCKER_DP=pgsql
export MOODLE_DOCKER_WEB_HOST=localhost

bin/moodle-docker-compose stop
```

```
# clean-moodle.sh (reset all configuration)
export MOODLE_DOCKER_WWWROOT="/path/to/the/moodle"
export MOODLE_DOCKER_DBTYPE=pgsql
export MOODLE_DOCKER_DP=pgsql
export MOODLE_DOCKER_WEB_HOST=localhost

bin/moodle-docker-compose down -v
```

`chmod +x start-moodle.sh stop-moodle.sh clean-moodle.sh` (executable)

modify db.pgsql.yml
```
services:
	webserver:
		environment:
			MOODLE_DOCKER_DBTYPE: pgsql
	db:
		image: postgres:${MOODLE_DOCKER_DB_VERSION:-14}
		environment:
			POSTGRES_USER: moodle
			POSTGRES_PASSWORD: "m@0dl3ing"
			POSTGRES_DB: moodle
		volumes:
			- pgdata:/var/lib/postgresql/data
volumes:
	pgdata:
```

## Starting
`./moodle-start.sh`

Will start localhost at port 8000, with fresh installation.


## IMPORTANT NOTES:
always use `stop-moodle.sh` for regular shutdowns to preserve data
`clean-moodle.sh` for fresh start
the database data will persist between starts and stops using the docker volume
http://localhost:8000 to access Moodle
Moodle code is in the `moodle` directory, which is mounted in Docker
Changes to the code are immediately reflected in the running instance

### Access database

```
docker exec -it moodle-docker-db-1 bash

psql -U moodle -d moodle
```

[Moodle 4.0 database schema web](https://www.examulator.com/er/4.0/)

Next step is to set up the [[IDE development environment]]