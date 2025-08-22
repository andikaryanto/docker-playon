FROM amazoncorretto:21-alpine3.18

RUN apk add --no-cache git \
    openssh

#copy idrsa
COPY config/sshkey/id_rsa /root/.ssh/id_rsa
RUN chmod 600 /root/.ssh/id_rsa

WORKDIR /Users/Shared/allstar

EXPOSE 8090

CMD ["tail", "-f", "/dev/null"]
