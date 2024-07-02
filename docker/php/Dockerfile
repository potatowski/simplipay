FROM php:8.2-fpm

RUN apt-get update && apt-get install -y \
    build-essential \
    autoconf \
    bison \
    re2c \
    libxml2-dev \
    libsqlite3-dev \
    libcurl4-openssl-dev \
    libpng-dev \
    libjpeg-dev \
    libonig-dev \
    libzip-dev \
    unzip \
    git \
    libicu-dev

RUN docker-php-ext-install -j$(nproc) intl
RUN docker-php-ext-install -j$(nproc) mbstring
RUN docker-php-ext-install -j$(nproc) pdo
RUN docker-php-ext-install -j$(nproc) pdo_mysql
RUN docker-php-ext-install -j$(nproc) xml
RUN docker-php-ext-install -j$(nproc) curl
RUN docker-php-ext-install -j$(nproc) gd
RUN docker-php-ext-install -j$(nproc) zip

RUN pecl install apcu \
    && docker-php-ext-enable apcu

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | bash \
    && apt-get install -y symfony-cli

WORKDIR /var/www/simplipay

COPY ./app /var/www/simplipay

ENV COMPOSER_ALLOW_SUPERUSER=1

RUN composer install