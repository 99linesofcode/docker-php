# syntax=docker/dockerfile:1

ARG VERSION="8.5"
ARG BASE_IMAGE="dunglas/frankenphp:php${VERSION}-alpine"

FROM ${BASE_IMAGE} AS base

LABEL org.opencontainers.image.authors="99linesofcode@gmail.com"

ARG VERSION

RUN apk add --no-cache \
    php${VERSION/./}-bcmath \
    php${VERSION/./}-ctype \
    php${VERSION/./}-curl \
    php${VERSION/./}-dom \
    php${VERSION/./}-fileinfo \
    php${VERSION/./}-fpm \
    php${VERSION/./}-iconv \
    php${VERSION/./}-intl \
    php${VERSION/./}-json \
    php${VERSION/./}-mbstring \
    php${VERSION/./}-opcache \
    php${VERSION/./}-openssl \
    php${VERSION/./}-pcntl \
    php${VERSION/./}-phar \
    php${VERSION/./}-pdo_mysql \
    php${VERSION/./}-pdo_pgsql \
    php${VERSION/./}-pdo_sqlite \
    php${VERSION/./}-session \
    php${VERSION/./}-simplexml \
    php${VERSION/./}-tokenizer \
    php${VERSION/./}-xml \
    php${VERSION/./}-xmlwriter \
    php${VERSION/./}-zlib \
    php${VERSION/./}-zip \
    php${VERSION/./}-pecl-redis;

WORKDIR /app

#
# Development
#

FROM base AS development

RUN apk add --no-cache \
    composer \
    php${VERSION/./}-pecl-xdebug;

COPY --link ./frankenphp/php.ini-development ${PHP_INI_DIR}/php.ini

#
# Production
#

FROM dunglas/frankenphp:builder AS caddy-builder

COPY --from=caddy:builder /usr/bin/xcaddy /usr/bin/xcaddy

# CGO must be enabled to build FrankenPHP
ENV CGO_ENABLED=1 \
    XCADDY_SETCAP=1 \
    XCADDY_GO_BUILD_FLAGS="-ldflags='-w -s' -tags=nobadger,nomysql,nopgx"

RUN xcaddy build \
    --output /usr/local/bin/frankenphp \
    --with github.com/dunglas/frankenphp=./ \
    --with github.com/dunglas/frankenphp/caddy=./caddy/ \
    --with github.com/caddy-dns/cloudflare;

FROM base AS composer-builder

RUN apk add --no-cache \
    composer && \
    composer install -n --no-cache --prefer-dist --no-dev --no-scripts -o -a;

FROM base AS production

RUN mv ${PHP_INI_DIR}/php.ini-production ${PHP_INI_DIR}/php.ini;

# Replace the official binary by the one containing our custom modules
COPY --from=caddy-builder /usr/local/bin/frankenphp /usr/local/bin/frankenphp
COPY --link ./frankenphp/Caddyfile /etc/caddy/Caddyfile
COPY --link --chmod=755 ./frankenphp/docker-php-entrypoint.sh /usr/local/bin/docker-php-entrypoint>

COPY . ./
