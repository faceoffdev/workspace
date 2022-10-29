ARG PHP_VERSION=81
FROM ghcr.io/faceoffdev/workspace/php:${PHP_VERSION}-dev

USER root

ARG PHP_VERSION=81

RUN apk add --no-cache --update \
    supercronic \
    php${PHP_VERSION}-posix \
    && rm -rf /tmp/* \
        /var/{cache,log}/* \
        /etc/crontabs/*

COPY rootfs /

RUN chown -R ${USER}:${USER} /etc/crontabs \
  && chown -R ${USER}:${USER} /etc/supervisor

USER ${USER}

ENV PHP_OPCACHE_VALIDATE_TIMESTAMPS=1 \
    PHP_OPCACHE_JIT_BUFFER_SIZE=0
