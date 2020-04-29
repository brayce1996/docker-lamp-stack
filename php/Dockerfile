FROM php:7.4.4-apache
COPY php.ini /usr/local/etc/php/php.ini
RUN docker-php-ext-install mysqli


# install packages
RUN apt-get update && apt-get install -y \
    git

# intall composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === 'e0012edf3e80b6978849f5eff0d4b4e4c79ff1609dd1e613307e16318854d24ae64f26d17af3ef0bf7cfb710ca74755a') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php --install-dir="/usr/bin" --filename=composer
RUN php -r "unlink('composer-setup.php');"

# asign github-oauth token
# YOU SHOULD PUT YOUR GITHUB TOKEN IN token/auth.json
COPY token/auth.json /root/.composer/

# install php packages
WORKDIR /php-packages
RUN composer require paquettg/php-html-parser

WORKDIR /var/www/html
