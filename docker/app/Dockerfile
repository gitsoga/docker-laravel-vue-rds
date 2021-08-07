# Node
FROM node:14.17.4-alpine as node

FROM php:7.4.1-fpm-alpine

RUN apk add --no-cache --virtual g++
RUN apk add --no-cache libstdc++ && apk add --no-cache libgcc

WORKDIR /var/www/html

# マルチステージビルドでphp-fpmコンテナにnodeとnpmを追加する
COPY --from=node /usr/local/bin/node /usr/local/bin/
COPY --from=node /usr/local/lib/node_modules/ /usr/local/lib/node_modules/
RUN ln -s /usr/local/bin/node /usr/local/bin/nodejs \
   &&   ln -s /usr/local/lib/node_modules/npm/bin/npm-cli.js /usr/local/bin/npm \
   &&   ln -s /usr/local/lib/node_modules/npm/bin/npm-cli.js /usr/local/bin/npx

RUN apk update \
    && apk add \
    git \
    zip \
    unzip \
    vim \
    && docker-php-ext-install pdo_mysql bcmath

# CORY php.ini
COPY ./docker/app/php.ini /usr/local/etc/php/php.ini

# Composer
COPY --from=composer /usr/bin/composer /usr/bin/composer

# 環境変数設定
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME "/opt/composer"
ENV PATH "$PATH:/opt/composer/vendor/bin"
