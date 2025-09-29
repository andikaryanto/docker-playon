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
    && echo "xdebug.mode = debug" >> /usr/local/etc/php/php.ini \
    && echo "xdebug.start_with_request = yes" >> /usr/local/etc/php/php.ini

# Install wp-cli
RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
    && chmod +x wp-cli.phar \
    && mv wp-cli.phar /usr/local/bin/wp

# Install gRPC extensions (unified versions)
RUN pecl install grpc-1.74.0 \
    && docker-php-ext-enable grpc

# clone grpc
RUN git clone -b v1.74.0 https://github.com/grpc/grpc /tmp/grpc \
    && cd /tmp/grpc \
    && git submodule update --init \
    && cd /tmp/grpc \
    && mkdir -p cmake/build \
    && cd cmake/build \
    && cmake ../.. \
    && make grpc_php_plugin \
    && cp grpc_php_plugin /usr/local/bin/

#clone protobuf
RUN git clone -b v25.2 https://github.com/protocolbuffers/protobuf /tmp/protobuf \
    && cd /tmp/protobuf \
    && git submodule update --init --recursive \
    && cd /tmp/protobuf \
    && cmake -S . -B build -Dprotobuf_BUILD_TESTS=OFF \
    && cmake --build build -j$(nproc) \
    && cmake --install build \
    && cp build/protoc /usr/bin/protoc \
    && ldconfig