#
# PHP 5.6.x Cli image
#

FROM php:5.6-cli

MAINTAINER Fabien D. <fabien@myprod.net>

RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ jessie-pgdg main" > /etc/apt/sources.list.d/pgdg.list \
    && curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add - \
    && apt-get update && apt-get install -y --no-install-recommends \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        libpq-dev \
        libzip-dev \
        libicu-dev \
        libxml2-dev \
        libssh2-1-dev \
        git \
        unzip \
    && rm -rf /var/lib/apt/lists/* \
    && pecl install -a ssh2-0.13 \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-configure pgsql --with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install -j$(nproc) mcrypt gd opcache mbstring \
    && docker-php-ext-install pdo pdo_mysql pdo_pgsql pgsql \
    && docker-php-ext-install zip bcmath intl exif soap \
    && docker-php-ext-enable ssh2

# Composer installation
RUN curl -sS https://getcomposer.org/installer | php \
    -- --install-dir=${COMPOSER_HOME:-/usr/local/bin} \
    --filename=composer

# PHP Application configuration (php.ini override)
ADD php-custom.ini /usr/local/etc/php/conf.d/

RUN usermod -u ${DOCKER_USER_ID:-1000} www-data \
    && mkdir -p ${APP_BASE_DIR:-/var/www/}

WORKDIR ${APP_BASE_DIR:-/var/www/}
