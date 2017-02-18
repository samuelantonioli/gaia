# TYPO3 Docker a.k.a "Gaia"

```
#docker #typo3 #cms #apache #php #mysql #mariadb #LAMP #alpine #linux
```

Dieses Repository enthält drei Dockerfiles,  
die ein einfaches Aufsetzen von  
TYPO3-Servern ermöglichen.

- typo3: Setzt einen Server mit TYPO3 auf
- server: Server mit Apache und PHP
- database: MySQL-Datenbank

Es wird [Alpine Linux](https://hub.docker.com/_/alpine/) genutzt, da es viel  
kleiner als andere Linux-Distributionen ist.

## Installation
Die Installation von `gaia` muss nur   
einmalig auf einem Computer durchgeführt werden.

```
git clone https://github.com/samuelantonioli/gaia.git && cd gaia
docker build -f database/Dockerfile -t gaia:database database
docker build -f server/Dockerfile -t gaia:server -t gaia server
docker build -f typo3/Dockerfile -t gaia:typo3 typo3
```

## TYPO3 aufsetzen

Der häufigste Anwendungsfall ist,  
dass man TYPO3 lokal und unkompliziert  
installieren möchte.  
Der `projektname` sollte überall mit dem  
Namen des Projektes ersetzt werden,  
wobei der Name nur aus Kleinbuchstaben,  
Zahlen und Unterstrichen bestehen darf.  
Die TYPO3-Installation befindet sich in  
der Instanz unter `/opt/typo3/src`.

### Setup
**Einmalig** für jede TYPO3-Instanz ausführen.

```
docker run --name projektname-db -e MYSQL_ROOT_PASSWORD=password -d gaia:database
docker run --name projektname -p 8080:80 -p 4443:443 --link projektname-db:db -d gaia:typo3
```

### TYPO3 nutzen
Nachdem man das Setup durchgeführt hat,  
führt man zum Starten und Beenden folgendes aus:

```
docker start projektname-db
docker start projektname
# Instanz über localhost:8080 erreichbar

# Zum Beenden der Instanz:
docker stop projektname
docker stop projektname-db
```

Installation TYPO3:  
Als Hostname der Datenbank einfach `db` eingeben.

### Instanz löschen
Vorher die Instanzen beenden, die man löschen möchte.  
**Damit wird die TYPO3-Instanz endgültig gelöscht!**

```
docker rm projektname-db
docker rm projektname
```

## Zugang zur Instanz

Wenn man auf eine Instanz zugreifen möchte,  
kann man darin eine Bash-Instanz starten.

```
docker exec -it containername bash
```

Mit `docker ps` kann man alle laufenden,  
mit `docker ps -a` alle vorhandenen  
Instanzen einsehen.

## Datenbank- oder Server-Instanz aufsetzen

Wenn man nicht TYPO3, sondern eine einzelne  
Datenbank- oder Server-Instanz benötigt, dann kann  
man die Images auch einzeln nutzen.

### Datenbank-Instanz
`dbname` mit anderem Namen ersetzen.

```
# Setup
docker run --name dbname -e MYSQL_ROOT_PASSWORD=password -d gaia:database
# Nutzen
docker start dbname
docker stop dbname
# Löschen
docker rm dbname
```

### Server-Instanz
Apache und PHP, `servername` mit anderem Namen ersetzen.  
Mit `-v` wird der Dateipfad vom Hostsystem in der Instanz  
gemounted.

```
# Setup
docker run --name servername -v /pfad/zu/deinen/dateien:/var/www/localhost/htdocs/ -d gaia:server
# Nutzen
docker start servername
docker stop servername
# Löschen
docker rm servername
```

### Beide nutzen

Wenn man Datenbank- und Server-Instanz  
gemeinsam verwenden möchte, funktioniert  
es wie bei *TYPO3 aufsetzen*.  
Ein einziger Unterschied beim Setup:

```
# :server statt :typo3
docker run --name projektname -p 8080:80 -p 4443:443 --link projektname-db:db -d gaia:server
```

## Credits

- [Datenbank](https://github.com/wangxian/alpine-mysql)
- [Apache & PHP](https://github.com/wichon/alpine-apache-php)
- [Apache](https://hub.docker.com/_/httpd/)

## Roadmap

- Standard-Konfigurationen definieren (PHP, Apache, SSL)
- `docker-compose` nutzen
- PHP-FPM nutzen
- Supervisor nutzen
