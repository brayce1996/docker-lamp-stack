FROM php:7.4.4-apache
COPY php.ini /usr/local/etc/php/php.ini
RUN docker-php-ext-install mysqli

# setup ssl
RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/ssl-cert-snakeoil.key -out /etc/ssl/certs/ssl-cert-snakeoil.pem -subj "/C=AT/ST=Vienna/L=Vienna/O=Security/OU=Development/CN=example.com"
RUN a2enmod rewrite
RUN a2ensite default-ssl
RUN a2enmod ssl
