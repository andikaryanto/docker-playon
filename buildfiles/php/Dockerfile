FROM php:7.4-fpm

# Set environment and user
ENV HOME /var/www
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

# Install Protocol Buffers (protoc)
ENV PROTOC_VERSION=3.20.3
RUN curl -LO https://github.com/protocolbuffers/protobuf/releases/download/v${PROTOC_VERSION}/protoc-${PROTOC_VERSION}-linux-x86_64.zip \
    && unzip protoc-${PROTOC_VERSION}-linux-x86_64.zip -d /usr/local \
    && rm protoc-${PROTOC_VERSION}-linux-x86_64.zip
    
# Install gRPC PHP extension
RUN pecl install grpc-1.43.0 \
    && docker-php-ext-enable grpc

# Install protoc and gRPC PHP plugin
RUN git clone --recurse-submodules -b v1.43.0 https://github.com/grpc/grpc /tmp/grpc \
    && cd /tmp/grpc \
    && make grpc_php_plugin \
    && cp bins/opt/grpc_php_plugin /usr/local/bin/ \
    && rm -rf /tmp/grpc

#copy idrsa
COPY config/sshkey/id_rsa /root/.ssh/id_rsa
RUN chmod 600 /root/.ssh/id_rsa

# Set working directory
WORKDIR /var/www

# Expose PHP-FPM port
EXPOSE 9000
