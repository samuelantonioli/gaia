FROM gaia:server
MAINTAINER Samuel Antonioli <contact@samuelantonioli.de>

RUN mkdir -p /var/www/localhost/htdocs && \
    cd /var/www/localhost/htdocs && \
    curl -L -o wp_src.tar.gz https://wordpress.org/latest.tar.gz && \
    tar -xzf wp_src.tar.gz --strip 1 && \
    rm -f wp_src.tar.gz

EXPOSE 80 443

CMD ["httpd-foreground"]
