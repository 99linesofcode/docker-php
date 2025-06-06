# syntax=docker/dockerfile:1

ARG PHP_VERSION="8.4"
ARG BASE_IMAGE="dunglas/frankenphp:php${PHP_VERSION}-alpine"

FROM ${BASE_IMAGE}

LABEL org.opencontainers.image.authors="99linesofcode@gmail.com"

WORKDIR /app

RUN set -eux; \
    install-php-extensions \
    @composer \
    bcmath \
    pdo_mysql \
    pdo_pgsql \
    intl \
    zip \
    xdebug;

COPY ./frankenphp/php.ini-development /usr/local/etc/php/php.ini

COPY . ./

CMD ["frankenphp", "run", "--config", "/etc/caddy/Caddyfile"]
