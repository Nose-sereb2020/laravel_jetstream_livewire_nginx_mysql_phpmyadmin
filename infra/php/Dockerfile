FROM php:7.4-fpm-buster
SHELL ["/bin/bash", "-oeux", "pipefail", "-c"]


RUN apt-get update && \
  apt-get -y install git unzip libzip-dev libicu-dev libonig-dev && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/* && \
  docker-php-ext-install intl pdo_mysql zip bcmath

COPY ./infra/php/php.ini /usr/local/etc/php/php.ini


ENV COMPOSER_ALLOW_SUPERUSER=1 \
  COMPOSER_HOME=/composer

COPY --from=composer:1.10 /usr/bin/composer /usr/bin/composer


RUN mkdir -p /work/storage/app/public
RUN mkdir -p /work/storage/framework/cache/data
RUN mkdir -p /work/storage/framework/sessions
RUN mkdir -p /work/storage/framework/testing
RUN mkdir -p /work/storage/framework/views
RUN chmod -R 777 /work/storage

COPY ./backend/composer.json /work/composer.json
COPY ./backend/composer.lock /work/composer.lock

RUN mkdir -p /work/database/seeds
RUN mkdir -p /work/database/factories


WORKDIR /work

RUN composer install --no-scripts