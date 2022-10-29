ARG PHP_VERSION=81
FROM ghcr.io/faceoffdev/workspace/php:${PHP_VERSION}

ENV PHP_FPM_TYPE=${PHP_FPM_TYPE} \
    PHP_FPM_MAX_CHILDREN=${PHP_FPM_MAX_CHILDREN} \
    PHP_FPM_START_SERVERS=${PHP_FPM_START_SERVERS} \
    PHP_FPM_MAX_SPARE_SERVERS=${PHP_FPM_MAX_SPARE_SERVERS} \
    PHP_FPM_MIN_SPARE_SERVERS=${PHP_FPM_MIN_SPARE_SERVERS} \
    PHP_FPM_REQUEST_TERMINATE_TIMEOUT=${PHP_FPM_REQUEST_TERMINATE_TIMEOUT:-60} \
    PHP_FPM_MAX_REQUESTS=${PHP_FPM_MAX_REQUESTS} \
    PHP_FPM_PROCESS_IDLE_TIMEOUT=${PHP_FPM_PROCESS_IDLE_TIMEOUT:-"10s"}

USER root

ARG PHP_VERSION=81

RUN apk add --no-cache --update \
    nginx \
    php${PHP_VERSION}-fpm \
    && ln -s /usr/sbin/php-fpm${PHP_VERSION} /usr/sbin/php-fpm

COPY rootfs /

RUN chown -R ${USER}:${USER} /var/log \
  && chown -R ${USER}:${USER} /var/lib \
  && chown -R ${USER}:${USER} /usr/lib \
  && chown -R ${USER}:${USER} /etc/php${PHP_VERSION}/conf.d

USER ${USER}

COPY --chown=${USER}:${USER} app ${WORKDIR}

EXPOSE 8080

HEALTHCHECK --timeout=10s CMD curl --silent --fail http://127.0.0.1:8080/fpm-ping
