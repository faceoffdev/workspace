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

ENV NEWRELIC_ENABLED=${NEWRELIC_ENABLED:-1} \
    NEWRELIC_LICENSE=${NEWRELIC_LICENSE} \
    NEWRELIC_APPNAME=${NEWRELIC_APPNAME:-"UNKNOWN"} \
    NEWRELIC_DAEMON_ADDRESS=${NEWRELIC_DAEMON_ADDRESS:-"np_newrelic_daemon:31339"} \
    NEWRELIC_TRANSACTION_TRACER_DETAIL=${NEWRELIC_TRANSACTION_TRACER_DETAIL:-0} \
    NEWRELIC_FRAMEWORK=${NEWRELIC_FRAMEWORK:-"laravel"} \
    NEWRELIC_RECORD_SQL=${NEWRELIC_RECORD_SQL:-"off"}

USER root

RUN apk add --no-cache --update \
    nginx \
    php${PHP_VERSION}-fpm

ARG NEWRELIC_AGENT_VERSION

RUN curl -L "https://download.newrelic.com/php_agent/archive/${NEWRELIC_AGENT_VERSION}/newrelic-php5-${NEWRELIC_AGENT_VERSION}-linux-musl.tar.gz" | tar -C /tmp -zx \
 && export NR_INSTALL_USE_CP_NOT_LN=1 \
 && export NR_INSTALL_SILENT=1 \
 && /tmp/newrelic-php5-*/newrelic-install install \
 && rm -rf /tmp/newrelic-php5-* /tmp/nrinstall*

COPY rootfs /

RUN chown -R ${USER}:${USER} /var/log \
  && chown -R ${USER}:${USER} /var/lib \
  && chown -R ${USER}:${USER} /usr/lib \
  && chown -R ${USER}:${USER} /etc/php${PHP_VERSION}/conf.d

USER ${USER}

COPY --chown=${USER}:${USER} app ${WORKDIR}

EXPOSE 8080

HEALTHCHECK --timeout=10s CMD curl --silent --fail http://127.0.0.1:8080/fpm-ping
