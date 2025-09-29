FROM andiaryanto11/playon-php:7.4-fpm.4

# Copy SSH key for private repos
COPY config/sshkey/id_rsa /root/.ssh/id_rsa
RUN chmod 600 /root/.ssh/id_rsa

# Set working directory
WORKDIR /var/www

# Expose PHP-FPM port   
EXPOSE 9000
