FROM andiaryanto11/playon-php:7.4-fpm

#install protoc
RUN cd /tmp/protobuf \
    && cmake -S . -B build -Dprotobuf_BUILD_TESTS=OFF \
    && cmake --build build -j$(nproc) \
    && cmake --install build \
    && cp build/protoc /usr/bin/protoc \
    && ldconfig

# Build and install gRPC PHP plugin
RUN cd /tmp/grpc \
    && mkdir -p cmake/build \
    && cd cmake/build \
    && cmake ../.. \
    && make grpc_php_plugin \
    && cp grpc_php_plugin /usr/local/bin/

# Copy SSH key for private repos
COPY config/sshkey/id_rsa /root/.ssh/id_rsa
RUN chmod 600 /root/.ssh/id_rsa

# Set working directory
WORKDIR /var/www

# Expose PHP-FPM port   
EXPOSE 9000
