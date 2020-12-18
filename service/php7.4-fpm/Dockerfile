FROM php:7.4-fpm

LABEL maintainer="David Pérez Terol <davidtxnet@gmail.com>"

ARG timezone=UTC

# Woking dir
WORKDIR /var/www/application

# Install latest Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('sha384', 'composer-setup.php') === '756890a4488ce9024fc62c56153228907f1545c228516cbf63f885e036d37e9a59d27d63f46af1d4d07ee0f76181c7d3') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php --install-dir=/usr/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"

# Install main packages
RUN apt-get update \
    && apt-get install -y \

    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \

    libonig-dev \
    libzip-dev \

    libmcrypt-dev \
    libxml2-dev \
    zlib1g \
    zlib1g-dev \
    libcurl3-dev \
    libicu-dev \
    sqlite3 \
    libsqlite3-dev \
    libmemcached-dev \
    zip \
    git \
    vim \
    wget

# PHP Extensions from source
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install \
    gd \
    curl \
    iconv \
    mbstring \
    bcmath \
    zip \
    pdo \
    pdo_mysql \
    intl \
    json \
    mysqli \
    pdo_sqlite \
    simplexml \
    sockets \
    xmlwriter \
    exif

# PHP Extends from PECL
RUN yes | pecl install xdebug-2.9.8
RUN docker-php-ext-enable xdebug

RUN pecl install redis-5.3.1
RUN docker-php-ext-enable redis

RUN pecl install memcached-3.1.5
RUN docker-php-ext-enable memcached

RUN pecl install msgpack-2.0.3
RUN docker-php-ext-enable msgpack

# Copy configuration files
COPY app.php.ini /usr/local/etc/php/conf.d/application.ini
COPY app.pool.conf /usr/local/etc/php-fpm.d/application.pool.conf

# Tweaks datetime (docker containers are in UTC)
RUN echo "${timezone}" > /etc/timezone && dpkg-reconfigure --frontend noninteractive tzdata
