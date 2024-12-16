FROM amazoncorretto:17-alpine3.18

RUN apk add --no-cache git \
    openssh

ENV MYSQL_DATABASE_HOST=docker-playon-mysql \
    MYSQL_DATABASE_USERNAME=root \
    MYSQL_DATABASE_NAME=core \
    MYSQL_DATABASE_PASSWORD= \
    MYSQL_DATABASE_PORT=3306 \
    LOG_LEVEL=INFO

WORKDIR /Users/Shared/allstar

EXPOSE 8090

CMD ["tail", "-f", "/dev/null"]
