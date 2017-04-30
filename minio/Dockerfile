FROM gmauro/armhf
MAINTAINER Gianmauro Cuccuru <gmauro@gmail.com>

#Permissions, set the linux user id and group id
ARG CONTAINER_UID=1000
ARG CONTAINER_GID=1000

ARG CONTAINER_USER=minio
ARG CONTAINER_GROUP=minio

ENV PATH $PATH:/home/$CONTAINER_USER/bin

# Add user
RUN addgroup -g $CONTAINER_GID $CONTAINER_GROUP && \
    adduser -u $CONTAINER_UID -G $CONTAINER_GROUP -h /home/$CONTAINER_USER -s /bin/bash -S $CONTAINER_USER && \
    mkdir -p /home/$CONTAINER_USER/bin && \
    apk add --update \
      bash \
      tini && \
    rm -rf /var/cache/apk/* && rm -rf /tmp/*

# https://dl.minio.io/server/minio/release/linux-arm/minio
ADD minio /home/$CONTAINER_USER/bin

ADD scripts/docker-entrypoint.sh /home/$CONTAINER_USER/bin

RUN chmod u+x /home/$CONTAINER_USER/bin/minio && \
    chmod +x /home/$CONTAINER_USER/bin/docker-entrypoint.sh

EXPOSE 9000

ENTRYPOINT ["/sbin/tini","--","/home/minio/bin/docker-entrypoint.sh"]

VOLUME ["/export"]

CMD ["minio"]