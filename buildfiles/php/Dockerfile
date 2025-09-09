FROM php:7.4-fpm

# Set environment and user
USER root

# Update and install required dependencies
RUN apt-get update && apt-get install -y \
    git \
    zip \
    unzip \
    zlib1g-dev \
    libzip-dev \
    libpng-dev \
    wget \
    bash \
    openssh-client \
    subversion \
    libssl-dev \
    autoconf \
    gcc \
    g++ \
    make \
    redis-tools \
    build-essential \
    libtool \
    pkg-config \
    cmake \
    linux-headers-amd64 \
    --no-install-recommends

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install Node.js and Yarn
RUN curl -fsSL https://deb.nodesource.com/setup_16.x | bash - \
    && apt-get install -y nodejs \
    && npm install -g yarn

# Enable and install PHP extensions
RUN docker-php-source extract \
    && docker-php-ext-install \
    pdo_mysql \
    gd \
    zip \
    bcmath \
    mysqli \
    sockets \
    pcntl \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && docker-php-source delete

# Install and configure Xdebug for PHP 7.4
RUN pecl install xdebug-3.1.6 \
    && docker-php-ext-enable xdebug \
    && echo "xdebug.mode = coverage" >> /usr/local/etc/php/php.ini

# Install wp-cli
RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
    && chmod +x wp-cli.phar \
    && mv wp-cli.phar /usr/local/bin/wp

# Install gRPC + Protobuf PHP extensions (unified versions)
# pecl install protobuf-3.21.12
RUN pecl install grpc-1.43.0 protobuf-3.21.12 \
    && docker-php-ext-enable grpc protobuf

# clone grpc
RUN git clone -b v1.43.0 https://github.com/grpc/grpc /tmp/grpc \
    && cd /tmp/grpc \
    && git submodule update --init

#clone protobuf
RUN git clone -b v27.3 https://github.com/protocolbuffers/protobuf /tmp/protobuf \
    && cd /tmp/protobuf \
    && git submodule update --init --recursive