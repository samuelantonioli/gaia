FROM alpine
MAINTAINER Samuel Antonioli <contact@samuelantonioli.de>

RUN apk --update add --no-cache apache2 bash vim tar ca-certificates curl ssmtp openssl \
    php7 php7-apache2 php7-phar php7-curl php7-soap \
    php7-json php7-zlib php7-xml php7-dom php7-ctype php7-opcache php7-zip php7-iconv \
    php7-pdo php7-pdo_mysql php7-pdo_sqlite php7-pdo_pgsql php7-mysqli php7-mbstring php7-session \
    php7-gd php7-mcrypt php7-openssl php7-sockets php7-posix php7-ldap php7-imap \
    apache2-utils shadow && \
    ln -s /usr/bin/php7 /usr/bin/php && \
    mkdir -p /run/apache2 && \
    sed -i 's/#LoadModule\ rewrite_module/LoadModule\ rewrite_module/' /etc/apache2/httpd.conf && \
    printf "php_value max_execution_time 300\nphp_value max_input_vars 3000\n" > /etc/apache2/conf.d/zzz.default.conf && \
    printf "php_value upload_max_filesize 60M\nphp_value post_max_size 64M\n" >> /etc/apache2/conf.d/zzz.default.conf && \
    printf "php_value memory_limit 256M\n" >> /etc/apache2/conf.d/zzz.default.conf && \
    printf "<Directory \"/var/www/localhost/htdocs\">\n\tAllowOverride All\n</Directory>\n" >> /etc/apache2/conf.d/zzz.default.conf && \
    echo '#!/bin/sh' > /usr/local/bin/httpd-foreground && \
    echo 'chown -R apache:apache /var/www/localhost/htdocs' >> /usr/local/bin/httpd-foreground && \
    echo 'rm -f /usr/local/apache2/logs/httpd.pid' >> /usr/local/bin/httpd-foreground && \
    echo 'httpd -DFOREGROUND' >> /usr/local/bin/httpd-foreground && \
    chmod +x /usr/local/bin/httpd-foreground && \
    usermod -u 1000 apache

EXPOSE 80 443

CMD ["httpd-foreground"]
