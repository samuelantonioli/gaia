FROM gaia:server
MAINTAINER Samuel Antonioli <contact@samuelantonioli.de>

# using HTTP 1.0 (without chunking support)
# because Sourceforge (TYPO3 filehost) is sending a wrong Content-Length
# curl (error #18)
RUN mkdir -p /opt/typo3/src && \
    mkdir -p /var/www/localhost/htdocs && \
    cd /opt/typo3/src && \
    curl -L -o typo3_src.tar.gz https://get.typo3.org/current --http1.0 && \
    tar -xzf typo3_src.tar.gz --strip 1 && \
    rm typo3_src.tar.gz && \
    cd /var/www/localhost/htdocs && \
    ln -s /opt/typo3/src typo3_src && \
    ln -s typo3_src/typo3 typo3 && \
    ln -s typo3_src/index.php index.php && \
    cp typo3_src/_.htaccess .htaccess && \
    touch FIRST_INSTALL && \
    rm -f /var/www/localhost/htdocs/index.html

EXPOSE 80 443

CMD ["httpd-foreground"]
