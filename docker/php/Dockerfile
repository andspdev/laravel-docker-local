FROM php:8.3-fpm-alpine

ARG UID=1000
ARG GID=1000

# System deps
RUN apk add --no-cache \
    bash \
    curl \
    git \
    unzip \
    icu-dev \
    oniguruma-dev \
    libzip-dev \
    linux-headers

RUN apk add --no-cache --virtual .build-deps \
    $PHPIZE_DEPS \
    autoconf \
    g++ \
    make

# PHP extensions
RUN docker-php-ext-install \
    pdo \
    pdo_mysql \
    mbstring \
    intl \
    zip

# Redis extension
RUN pecl install redis \
 && docker-php-ext-enable redis

 RUN apk del .build-deps

# Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# User (lokal = 1000)
RUN addgroup -g ${GID} app \
 && adduser -D -G app -u ${UID} app

WORKDIR /app

# Permission Laravel
RUN mkdir -p storage bootstrap/cache \
 && chown -R app:app /app

USER app
