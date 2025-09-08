FROM andiaryanto11/playon-php:7.4-fpm

# Build and install gRPC PHP plugin
RUN git clone -b v1.43.0 https://github.com/grpc/grpc /tmp/grpc \
    && cd /tmp/grpc \
    && git submodule update --init \
    && mkdir -p cmake/build \
    && cd cmake/build \
    && cmake ../.. \
    && make grpc_php_plugin \
    && cp bins/opt/grpc_php_plugin /usr/local/bin/ \
    && rm -rf /tmp/grpc

# Copy SSH key for private repos
COPY config/sshkey/id_rsa /root/.ssh/id_rsa
RUN chmod 600 /root/.ssh/id_rsa

# Set working directory
WORKDIR /var/www

# Expose PHP-FPM port   
EXPOSE 9000
