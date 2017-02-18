FROM alpine
MAINTAINER Samuel Antonioli <contact@samuelantonioli.de>

COPY run-mysql /usr/local/bin/run-mysql
RUN chmod +x /usr/local/bin/run-mysql && \
    apk --update add --no-cache bash vim mysql mysql-client
COPY my.cnf /etc/mysql/my.cnf

EXPOSE 3306

CMD ["run-mysql"]
