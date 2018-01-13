# TYPO3 Docker a.k.a "Gaia"

```
#docker #typo3 #cms #apache #php #mysql #mariadb #LAMP #alpine #linux
```

This repository contains three Dockerfiles  
that enable a simple installation of TYPO3 servers.

- typo3: server with TYPO3
- server: server with apache und php
- database: MySQL database

The Dockerfiles are based on [Alpine Linux](https://hub.docker.com/_/alpine/)
because it's much smaller than other linux distributions.

## Installation

The installation of `gaia` only needs to happen once.

```
git clone https://github.com/samuelantonioli/gaia.git && cd gaia
docker build -f database/Dockerfile -t gaia:database database
docker build -f server/Dockerfile -t gaia:server -t gaia server
docker build -f typo3/Dockerfile -t gaia:typo3 typo3
```

## TYPO3 setup

A common use case is to install TYPO3 locally and uncomplicated.
The `projectname` should be replaced everywhere with your project
name in the following shell commands.
It should only contain letters, numbers, dashes and underscores.  

The TYPO3 installation can be found
under `/opt/typo3/src`.

### Setup
**Only once** for each TYPO3 installation:

```
docker run --name projectname-db -e MYSQL_ROOT_PASSWORD=password -d gaia:database
docker run --name projectname -p 8080:80 -p 4443:443 --link projectname-db:db -d gaia:typo3
```

### Using TYPO3
After the installation,
it can be started and closed with the following commands:

```
docker start projectname-db
docker start projectname
# instance can be found at localhost:8080

# stop the instance
docker stop projectname
docker stop projectname-db
```

While installing TYPO3:  
Use `db` as hostname for the database.

### Delete TYPO3 installation
Make sure you've stopped the instance before.  
**Be careful! The installation will be deleted permanently!**

```
docker rm projectname-db
docker rm projectname
```

## Access the docker container

It is possible to open a bash in the docker container:

```
docker exec -it containername bash
```

You can see all running docker instances using `docker ps`.  
You can see all instances with `docker ps -a`.

## Database or server setup

You can also use the images independently,
i.e. you can start database and server instances
without using TYPO3.

### Database setup
Replace `dbname` with the name you want to use.

```
# Setup
docker run --name dbname -e MYSQL_ROOT_PASSWORD=password -d gaia:database
# Use it
docker start dbname
docker stop dbname
# Delete
docker rm dbname
```

### Server setup
Apache and PHP7. Replace `servername` with the name you want to use.  
You can mount a directory from your host system in your docker instance using `-v`.

```
# Setup
docker run --name servername -v /path/to/your/files:/var/www/localhost/htdocs/ -d gaia:server
# Use
docker start servername
docker stop servername
# Delete
docker rm servername
```

### Using both

If you want to use the database and the server together e.g. for Wordpress,
you can do the following (similar to *TYPO3 setup*):

```
# :server instead of :typo3
docker run --name projectname -p 8080:80 -p 4443:443 --link projectname-db:db -d gaia:server
```

## Credits

- [MySQL](https://github.com/wangxian/alpine-mysql)
- [Apache & PHP](https://github.com/wichon/alpine-apache-php)
- [Apache](https://hub.docker.com/_/httpd/)

## Roadmap

- Defining standard configurations (PHP, Apache, SSL)
- use `docker-compose`
- use PHP-FPM
- use a Supervisor
