FROM php:7.4-fpm-alpine

ENV HOME /www-data
USER root

RUN apk add --no-cache \
    $PHPIZE_DEPS \
    openssl-dev \
    autoconf \
    linux-headers \
    libtool \
    gcc \
    make

RUN  apk --update add git zip zlib-dev grep libzip-dev libpng-dev wget  bash openssh-client subversion

RUN docker-php-source extract \
    && docker-php-ext-install pdo_mysql gd zip bcmath mysqli \
    # Install Redis extension using PECL
    && pecl install redis \
    && docker-php-ext-enable redis \
    && docker-php-source delete   
    
# needed by xdebug

# RUN pecl install xdebug \
#     && docker-php-ext-enable xdebug
    
WORKDIR /var/www

EXPOSE 9000