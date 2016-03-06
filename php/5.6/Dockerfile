#
# PHP 5.6.x FPM image
#

FROM php:5.6-fpm

MAINTAINER Fabien D. <fabien@myprod.net>

RUN apt-get update && apt-get install -y --no-install-recommends \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        libpq-dev \
        libzip-dev \
        git \
        unzip \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) mcrypt gd opcache mbstring \
    && docker-php-ext-install pdo pdo_mysql pdo_pgsql \
    && docker-php-ext-install zip bcmath

# Composer installation
RUN curl -sS https://getcomposer.org/installer | php \
    -- --install-dir=${COMPOSER_HOME:-/usr/local/bin} \
    --filename=composer

# PHP Application configuration (php.ini override + FPM pool configuration)
ADD php-custom.ini /usr/local/etc/php/conf.d/
ADD symfony.pool.conf /etc/php5/fpm/pool.d/

RUN usermod -u ${DOCKER_USER_ID:-1000} www-data \
    && mkdir -p ${APP_BASE_DIR:-/var/www/}

WORKDIR ${APP_BASE_DIR:-/var/www/}