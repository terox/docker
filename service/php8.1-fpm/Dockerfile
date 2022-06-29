FROM php:8.1-fpm
LABEL maintainer="David Pérez Terol <davidtxnet@gmail.com>"

ARG timezone=UTC

# Woking dir
WORKDIR /var/www/application

# Install latest Composer
COPY composer-installer.sh .
RUN sh composer-installer.sh

# Install main packages
RUN apt-get update \
    && apt-get install -y \

    # Image and fonts manipulation
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libwebp-dev \

    # Database
    sqlite3 \
    libpq-dev \
    libsqlite3-dev \

    # Data structures
    libxml2-dev \

    # Tools
    libonig-dev \
    libzip-dev \
    libmcrypt-dev \
    zlib1g \
    zlib1g-dev \
    libcurl3-dev \
    libicu-dev \
    zip \
    git \
    vim \
    wget

# PHP Extensions from source
RUN docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp --with-freetype

RUN docker-php-ext-install \

    # Image and fonts manipulation
    gd \
    exif \

    # Data bases
    pdo_mysql \
    pdo_pgsql \
    pdo_sqlite \
    mysqli \

    # Data structures
    simplexml \
    xmlwriter \

    # Communications
    sockets \

    # Tools
    intl \
    curl \
    iconv \
    mbstring \
    bcmath \
    zip

# PHP Extends from PECL
RUN yes | pecl install xdebug-3.1.5
RUN docker-php-ext-enable xdebug

RUN pecl install redis-5.3.7
RUN docker-php-ext-enable redis

RUN pecl install msgpack-2.1.2
RUN docker-php-ext-enable msgpack

# Copy configuration files
COPY app.php.ini /usr/local/etc/php/conf.d/application.ini
COPY xdebug.php.ini /usr/local/etc/php/conf.d/xdebug.ini
COPY app.pool.conf /usr/local/etc/php-fpm.d/application.pool.conf

# Tweaks datetime (docker containers are in UTC)
RUN echo "${timezone}" > /etc/timezone && dpkg-reconfigure --frontend noninteractive tzdata
