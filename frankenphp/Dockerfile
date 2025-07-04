# syntax=docker/dockerfile:1

ARG PHP_VERSION="8.4"
ARG BASE_IMAGE="dunglas/frankenphp:php${PHP_VERSION}-alpine"

FROM ${BASE_IMAGE} AS base

LABEL org.opencontainers.image.authors="99linesofcode@gmail.com"

RUN set -eux; \
    install-php-extensions \
    bcmath \
    pdo_mysql \
    pdo_pgsql \
    intl \
    zip \
    redis;

WORKDIR /app

#
# Development
#

FROM base AS development

RUN set -eux; \
    install-php-extensions \
    @composer \
    xdebug;

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

RUN set -eux; \
    xcaddy build \
    --output /usr/local/bin/frankenphp \
    --with github.com/dunglas/frankenphp=./ \
    --with github.com/dunglas/frankenphp/caddy=./caddy/ \
    --with github.com/caddy-dns/cloudflare;

FROM base AS composer-builder

RUN set -eux; \
    install-php-extensions \
    @composer \
    && composer install -n --no-cache --prefer-dist --no-dev --no-scripts -o -a;

FROM base AS production

RUN set -eux; \
    mv ${PHP_INI_DIR}/php.ini-production ${PHP_INI_DIR}/php.ini;

# Replace the official binary by the one containing our custom modules
COPY --from=caddy-builder /usr/local/bin/frankenphp /usr/local/bin/frankenphp
COPY --link ./frankenphp/Caddyfile /etc/caddy/Caddyfile
COPY --link --chmod=755 ./frankenphp/docker-php-entrypoint.sh /usr/local/bin/docker-php-entrypoint>

COPY . ./
