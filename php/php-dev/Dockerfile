ARG PHP_VERSION=81
FROM ghcr.io/faceoffdev/workspace/php:${PHP_VERSION}

ENV XDEBUG_CLIENT_HOST=${XDEBUG_CLIENT_HOST:-172.17.0.1}

USER root

ARG PHP_VERSION=81

RUN apk add --no-cache --update \
        php${PHP_VERSION}-gd \
        php${PHP_VERSION}-xml \
        php${PHP_VERSION}-xmlwriter \
        php${PHP_VERSION}-xmlreader \
        php${PHP_VERSION}-pecl-xdebug

RUN find /etc/php${PHP_VERSION}/conf.d/*_xdebug.ini -delete \
  && echo "zend_extension=xdebug.so" >> /etc/php${PHP_VERSION}/conf.d/00_xdebug.ini

COPY rootfs /

USER ${USER}
