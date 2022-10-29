ARG PHP_VERSION=81
FROM ghcr.io/faceoffdev/workspace/php:${PHP_VERSION}-dev

USER root

ARG PHP_VERSION=81

RUN apk add --no-cache --update \
    nginx \
    php${PHP_VERSION}-fpm \
    && ln -s /usr/sbin/php-fpm${PHP_VERSION} /usr/sbin/php-fpm

COPY rootfs /

RUN chown -R ${USER}:${USER} /var/log \
  && chown -R ${USER}:${USER} /var/lib

USER ${USER}

COPY --chown=${USER}:${USER} app ${WORKDIR}

ENV PHP_OPCACHE_VALIDATE_TIMESTAMPS=1 \
    PHP_OPCACHE_JIT_BUFFER_SIZE=0

EXPOSE 8080

HEALTHCHECK --timeout=10s CMD curl --silent --fail http://127.0.0.1:8080/fpm-ping
