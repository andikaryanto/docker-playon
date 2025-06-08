FROM amazoncorretto:21-alpine3.18

RUN apk add --no-cache git \
    openssh

#copy idrsa
COPY config/sshkey/id_rsa /root/.ssh/id_rsa
RUN chmod 600 /root/.ssh/id_rsa

ENV MYSQL_DATABASE_HOST=docker-playon-mysql \
    MYSQL_DATABASE_USERNAME=root \
    MYSQL_DATABASE_NAME=core \
    MYSQL_DATABASE_PASSWORD= \
    MYSQL_DATABASE_PORT=3306 \
    LOG_LEVEL=INFO

WORKDIR /Users/Shared/allstar

EXPOSE 8090

CMD ["tail", "-f", "/dev/null"]
