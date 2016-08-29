FROM php:5.6-fpm
COPY customized.ini /usr/local/etc/php/conf.d/customized.ini
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        libxml2-dev \
        libcurl4-openssl-dev \
    && docker-php-ext-install -j$(nproc) iconv mcrypt curl dom hash pdo pdo_mysql simplexml soap \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && pecl install redis-2.2.8 \
    && pecl install xdebug \
    && docker-php-ext-enable redis
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y ssmtp
COPY ssmtp.conf /etc/ssmtp/ssmtp.conf
COPY revaliases /etc/ssmtp/revaliases
