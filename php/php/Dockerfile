FROM alpine:latest

ENV PHP_POST_MAX_SIZE=${PHP_POST_MAX_SIZE:-"8M"} \
    PHP_UPLOAD_MAX_FILESIZE=${PHP_UPLOAD_MAX_FILESIZE:-"2M"} \
    PHP_MEMORY_LIMIT=${PHP_MEMORY_LIMIT:-"256M"}

ENV PHP_OPCACHE_MAX_ACCELERATED_FILES=${PHP_OPCACHE_MAX_ACCELERATED_FILES:-16229} \
    PHP_OPCACHE_ENABLE_CLI=${PHP_OPCACHE_ENABLE_CLI:-0} \
    PHP_OPCACHE_MEMORY_CONSUMPTION=${PHP_OPCACHE_MEMORY_CONSUMPTION:-256} \
    PHP_OPCACHE_VALIDATE_TIMESTAMPS=${PHP_OPCACHE_VALIDATE_TIMESTAMPS:-0} \
    PHP_OPCACHE_JIT_BUFFER_SIZE=${PHP_OPCACHE_JIT_BUFFER_SIZE:-"128M"} \
    PHP_OPCACHE_INTERNED_STRINGS_BUFFER=${PHP_OPCACHE_INTERNED_STRINGS_BUFFER:-64}

ARG PHP_VERSION=81

RUN apk add --no-cache --update \
    php${PHP_VERSION} \
    php${PHP_VERSION}-phar \
    php${PHP_VERSION}-openssl \
    php${PHP_VERSION}-ctype \
    php${PHP_VERSION}-bcmath \
    php${PHP_VERSION}-mbstring \
    php${PHP_VERSION}-json \
    php${PHP_VERSION}-opcache \
    php${PHP_VERSION}-fileinfo \
    php${PHP_VERSION}-curl \
    php${PHP_VERSION}-redis \
    php${PHP_VERSION}-tokenizer \
    php${PHP_VERSION}-dom \
    php${PHP_VERSION}-simplexml \
    php${PHP_VERSION}-iconv \
    php${PHP_VERSION}-zip \
    php${PHP_VERSION}-pdo_pgsql \
    php${PHP_VERSION}-pdo_mysql \
    php${PHP_VERSION}-pcntl \
    tzdata \
    tini \
    supervisor \
    curl \
    nano \
    git \
    bash \
    && mkdir -p /app \
    && rm -rf /tmp/* \
    /var/{cache,log}/* \
    && ln -s /usr/bin/php${PHP_VERSION} /usr/bin/php

COPY rootfs /

ARG UID=1000
ARG GID=1000
ENV USER='application'
ENV WORKDIR='/app'
ENV TZ=Europe/Moscow
ARG GITHUB_TOKEN_PUBLIC

RUN addgroup --gid ${GID} --system ${USER} \
    && adduser --uid ${UID} --system --ingroup ${USER} ${USER} \
    && chown -R ${USER}:${USER} ${WORKDIR} /home/${USER}

RUN chown -R ${USER}:${USER} /app \
    && chown -R ${USER}:${USER} /run

USER ${USER}

WORKDIR ${WORKDIR}

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN composer config -g github-oauth.github.com ${GITHUB_TOKEN_PUBLIC}

RUN bash -c "$(curl -fsSL https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh)" && \
  sed -i 's/OSH_THEME="font"/OSH_THEME="agnoster"/' ~/.bashrc

ENTRYPOINT ["/sbin/tini", "--", "/usr/bin/docker-entrypoint.sh"]

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisord.conf"]
