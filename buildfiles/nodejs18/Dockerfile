FROM node:18

# Install Git
RUN apt-get update && apt-get install -y git && apt-get clean

# Setup SSH
RUN mkdir -p /root/.ssh && chmod 700 /root/.ssh
COPY config/sshkey/id_rsa /root/.ssh/id_rsa
RUN chmod 600 /root/.ssh/id_rsa

# Set working directory
WORKDIR /projects

# Expose port
EXPOSE 4010

# Keep container running
CMD ["tail", "-f", "/dev/null"]
